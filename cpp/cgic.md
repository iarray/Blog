## CGIC

### CGIC相关知识
1. 从[官网](http://www.boutell.com/cgic/#obtain)获取CGIC源程序。
2. 解压后，其中文件如下：
	* Cgic.h：头文件；
	* Cgic.c：CGIC的源代码文件；
	* Cgictest.c：CGIC库的作者提供的一个CGI程序例子；
	* Capture.c：用于调试CGI程序的工具；
	* Makefile：安装CGIC的脚本文件；

整个CIGC实际上是一个CGIC.C一个文件，非常精炼。只要在编写CGI程序时，将CGIC.C和CGIC.H两个文件放到当前文件夹下，代码中加入 `#include "cgic.h"` 进行编译就可以了。
运用CGIC来编写CGI程序，大大简化了直接用C来编写CGI程序的过程。它解决了CGI程序和HTML互交的问题。

3. 使用CGIC之后，CGI程序的编写不再是由Main()开始，而是由`cgiMain()`开始。
4. 输出内容,基本使用 `fprintf(cgiOut,"{\"errcode\":0}")` 其中的cgiOut实际上就是stdin。
5. 读取表单POST数据 
	
		char username[32] = "";
		cgiFormString("user", username, sizeof(username));
6. 获取Get请求字符串
	
	* 在你编写的cgiMain方法前面加入以下声明：
	* extern char *cgiQueryString;

			#include "cgic.h"
			#include <stdio.h>
			#include <string.h>
			#include <stdlib.h>
			
			extern char *cgiQueryString;
			
			int cgiMain() {
				cgiHeaderContentType("text/html");
				fprintf(cgiOut, "<HTML><HEAD>\n");
				fprintf(cgiOut, "<H1>%s</H1>",cgiQueryString);
				fprintf(cgiOut, "</BODY>\n");
				fprintf(cgiOut, "</HTML>\n");
				return 0;
			}

7. 字符串反转义 `cgiResultType cgiChars(char **sp, char *cp, int len);`  注意第一个参数sp在方法内部分配了内存空间,使用完后应该`free`掉
8. 上传文件

		#include <stdio.h>
		#include<stdlib.h>
		#include <string.h>
		#include <unistd.h>
		#include <fcntl.h>
		#include <sys/stat.h> 
		#include "cgic.h" 
		
		#define BufferLen 1024
		 
		int cgiMain(void)
		{
		    cgiFilePtr file;
		    int targetFile;
		    mode_t mode;
		    char name[128];
		    char fileNameOnServer[64];
		    char contentType[1024];
		    char buffer[BufferLen];
		    char *tmpStr = NULL;
		    int size;
		    int got, t;
		    cgiHeaderContentType("text/html");
		    //取得html页面中file元素的值，应该是文件在客户机上的路径名
		    if (cgiFormFileName("file", name, sizeof(name)) != cgiFormSuccess)
		    {
		        fprintf(stderr, "could not retrieve filename\r\n");
		        goto FAIL;
		    }
		    cgiFormFileSize("file", &size);
		    //取得文件类型，不过本例中并未使用
		    cgiFormFileContentType("file", contentType, sizeof(contentType));
		    //目前文件存在于系统临时文件夹中，通常为/tmp，通过该命令打开临时文件。临时文件的名字与用户文件的名字不同，所以不能通过路径/tmp/userfilename的方式获得文件
		    if (cgiFormFileOpen("file", &file) != cgiFormSuccess)
		    {
		        fprintf(stderr, "could not open the file\r\n");
		        goto FAIL;
		    }
		    t = -1;
		    //从路径名解析出用户文件名
		    while (1)
		    {
		        tmpStr = strstr(name + t + 1, "//");
		        if (NULL == tmpStr)
		            tmpStr = strstr(name + t + 1, "/"); //if "//" is not path separator, try "/"
		        if (NULL != tmpStr)
		            t = (int)(tmpStr - name);
		        else
		            break;
		    }
		    strcpy(fileNameOnServer, name + t + 1);
		    mode = S_IRWXU | S_IRGRP | S_IROTH;
		    //在当前目录下建立新的文件，第一个参数实际上是路径名，此处的含义是在cgi程序所在的目录（当前目录））建立新文件
		    targetFile = open(fileNameOnServer, O_RDWR | O_CREAT | O_TRUNC | O_APPEND, mode);
		    if (targetFile < 0)
		    {
		        fprintf(stderr, "could not create the new file, %s\r\n", fileNameOnServer);
		        goto FAIL;
		    }
		    //从系统临时文件中读出文件内容，并放到刚创建的目标文件中
		    while (cgiFormFileRead(file, buffer, BufferLen, &got) == cgiFormSuccess)
		    {
		        if (got > 0)
		            write(targetFile, buffer, got);
		    }
		    cgiFormFileClose(file);
		    close(targetFile);
		    goto END;
		FAIL:
		    fprintf(stderr, "Failed to upload\r\n");
		    return 1;
		END:
		    printf("File %s has been uploaded", fileNameOnServer); 
		
		    return 0;
		}

文件上传的大小在cgic.c源码中设置,源码中变量cgiContentLength定义了请求的长度.