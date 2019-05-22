```java

package com.soben.hardware;

import com.soben.hardware.tools.ShellTools;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/**
 * 全志Gpio操作
 * PH6_UART3-RTS_LS-INT：红外检测，检测到有人打开液晶屏与摄像头进行人脸识别；
 * PB5_PCM0-BCLK 红外补光灯
 * PB6_PCM0-DOUT：白光灯控制IO口；
 * PB2_UART2_RTS门磁检测IO口、
 * PB3_UART2_CTS：出门按钮检测IO口；
 * PL9_BB-PWRON：开锁继电器控制口；龚总给下这些io口的调用事例文档
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
