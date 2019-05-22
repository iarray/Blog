```java

package com.soben.hardware;

import com.soben.hardware.tools.ShellTools;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/**
 * 全志Gpio操作
 */
public class AllwinnerGpio {


    public static void writeGpio(String gpio, int value) {//value为0则输出低电平，1输出高电平
        FileWriter fw = null;
        try {
            fw = new FileWriter("/sys/class/gpio_sw/" + gpio + "/data", true);
            fw.write(Integer.toString(value));
        } catch (IOException e) {
            e.printStackTrace();
        }finally{
            if (fw != null) {
                try {
                    fw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public static int readGpio(String gpio) {//value为0则输出低电平，1输出高电平
        FileReader fr = null;
        try {
            fr = new FileReader("/sys/class/gpio_sw/" + gpio + "/data");
            return fr.read() - 48;
        } catch (IOException e) {
            e.printStackTrace();
            return -1;
        }finally{
            if (fr != null) {
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}


```

调用实例
```java

//PL9输出高电平
AllwinnerGpio.writeGpio("PL9", 1);


```
