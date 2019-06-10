## 创建任务
```c
portBASE_TYPE xTaskCreate( pdTASK_CODE pvTaskCode,
	const signed portCHAR * const pcName,
        unsigned portSHORT usStackDepth,
        void *pvParameters,
        unsigned portBASE_TYPE uxPriority,
        xTaskHandle *pxCreatedTask );
```

参数名|描述
-|-
pvTaskCode|参数pvTaskCode 是一个指向任务的实现函数的指针(任务是永不退出的C函数，实现常通常是一个死循环。)
pcName|具有描述性的任务名。这个参数不会被 FreeRTOS 使用。其只是单 纯地用于辅助调试。识别一个具有可读性的名字总是比通过句柄来 识别容易得多。
usStackDepth|当任务创建时，内核会分为每个任务分配属于任务自己的唯一状态。 usStackDepth 值用于告诉内核为它分配多大的栈空间。<br>这个值指定的是栈空间可以保存多少个字(word)，而不是多少个字节(byte)。比如说，如果是32位宽的栈空间，传入的 usStackDepth值为100，则将会分配400字节的栈空间(100 * 4bytes)。栈深度乘以栈宽度的结果千万不能超过一个 size_t 类型变量所能表达的最大值。
pvParameters|pvParameters 的值即是传递到任务中的值(void指针(void*))
uxPriority|指定任务执行的优先级。优先级的取值范围可以从最低优先级 0 到 最高优先级(configMAX_PRIORITIES – 1)。<br>configMAX_PRIORITIES 是一个由用户定义的常量。优先级号并没有上限(除了受限于采用的数据类型和系统的有效内存空间)，但最好使用实际需要的最小数值以避免内存浪费。如果 uxPriority 的值超过了(configMAX_PRIORITIES – 1)，将会导致实际赋给任务的优先级被自动封顶到最大合法值。
pxCreatedTask|pxCreatedTask 用于传出任务的句柄。这个句柄将在API调用中对该创建出来的任务进行引用，比如改变任务优先级，或者删除任务。<br>如果应用程序中不会用到这个任务的句柄，则 pxCreatedTask 可以被设为 NULL。
返回值|有两个可能的返回值:<br>1. pdTRUE表明任务创建成功。<br>2. errCOULD_NOT_ALLOCATE_REQUIRED_MEMORY 由于内存堆空间不足，FreeRTOS 无法分配足够的空间来保存任务结构数据和任务栈，因此无法创建任务。

### 示例
```c
void vTaskFunction( void *pvParameters )
{
  char *pcTaskName;
  volatile unsigned long ul;
  pcTaskName = ( char * ) pvParameters;
  /* 和大多数任务一样，该任务处于一个死循环中。 */ 
  for( ;; )
  {
  } 
  /* Print out the name of this task. */
  vPrintString( pcTaskName );
  /* 延迟，以产生一个周期 */
  for( ul = 0; ul < mainDELAY_LOOP_COUNT; ul++ ) {
  /* 这个空循环是最原始的延迟实现方式。在循环中不做任何事情。后面的示例程序将采用
  delay/sleep函数代替这个原始空循环。 */ 
  }
}

int main( void )
{
static const char *pcTaskName1 = "Task 1 is running\r\n";
static const char *pcTaskName2 = "Task 2 is running\r\n";

/* 创建第一个任务。需要说明的是一个实用的应用程序中应当检测函数xTaskCreate()的返回值，以确保任 务创建成功。 */
  xTaskCreate(
    vTaskFunction, /* 指向任务函数的指针. */ 
    "Task 1", /* 任务名. */
    1000, /* 栈深度. */
    (void*)pcTaskName1, /* 通过任务参数传入需要打印输出的文本. */ 
    1, /* 此任务运行在优先级1上. */
    NULL /* 不会用到此任务的句柄. */
  );

  xTaskCreate( vTask2, "Task 2", 1000, (void*)pcTaskName2, 1, NULL );
  /* 启动调度器，任务开始执行 */ 
  vTaskStartScheduler();
  /* 如果一切正常，main()函数不应该会执行到这里。但如果执行到这里，很可能是内存堆空间不足导致空闲 任务无法创建。第五章有讲述更多关于内存管理方面的信息 */
  for( ;; );
}
```
### 输出
```txt
Task 1 is running
Task 2 is running
Task 1 is running
Task 2 is running
Task 1 is running
Task 2 is running
```


