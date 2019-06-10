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
pvTaskCode|任务只是永不退出的 C 函数，实现常通常是一个死循环。参数 pvTaskCode 只一个指向任务的实现函数的指针(效果上仅仅是函数 名)。