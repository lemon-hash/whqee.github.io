---
layout: post
title: printf重定向，支持浮点（stm32）
date: 2019-03-02 18:55:00 +800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img:  # Add image post (optional)
fig-caption: # Add figcaption (optional)
categories: [Blogging]
tags: [stm32,嵌入式] # add tag
---

## 添加如下代码：
{% highlight bash %}
#include "stdio.h"
#include "stdlib.h"

#ifdef __GNUC__
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch)
#else
#define PUTCHAR_PROTOTYPE int fputc(int ch, FILE *f)
#endif

#define USARTx huart1

PUTCHAR_PROTOTYPE
{
    HAL_UART_Transmit(&USARTx , (uint8_t *)&ch, 1 , 0xffff);

//    while ((USART1->SR & 0X40) == 0); //寄存器操作,循环发发送,发完为止
//    USART1->DR = (uint8_t) ch;
    return ch;
}
{% endhighlight %}


{% highlight bash %}
int _write(int fd, char *ptr, int len);
void print_float(float value);

int _write(int fd, char *ptr, int len) 
{ 
    int i = 0; 
    /*
     * write "len" of char from "ptr" to file id "fd"
     * Return number of char written.
     *
    * Only work for STDOUT, STDIN, and STDERR
     */ 
    if (fd > 2) { return -1; } 
    while (*ptr && (i < len)) 
    { 
    HAL_UART_Transmit(&USARTx,ptr,sizeof(*ptr),10);
    if (*ptr == '\n') 
    { 
        HAL_UART_Transmit(&USARTx,(uint8_t*)"\r",2,10);
    } 
    i++; 
    ptr++; 
    } 
    return i; 
}

void print_float(float value)
{

  int tmp,tmp1,tmp2,tmp3;

  tmp = (int)value;

  tmp1=(int)((value-tmp)*10)%10;

  tmp2=(int)((value-tmp)*100)%10;

  tmp3=(int)((value-tmp)*1000)%10;

  printf("%d.%d%d%d\n", tmp,tmp1,tmp2,tmp3);

}
{% endhighlight %}