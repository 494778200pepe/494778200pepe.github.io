---
layout: post
title:  "return与finally"
date:   2019-07-14 16:25:00 +0800
categories: Java
tags: Java基础
author: pepe
description: 『 return与finally 』
---

有return的情况下try catch finally的执行顺序

### 结论：

* 1、不管有没有出现异常，finally块中代码都会执行；

* 2、当try和catch中有return时，finally仍然会执行；

* 3、finally是在return后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来，不管finally中的代码怎么样，返回的值都不会改变，任然是之前保存的值），所以函数返回值是在finally执行前确定的；

* 4、finally中最好不要包含return，否则程序会提前退出，返回值不是try或catch中保存的返回值。

*举例：*

* 情况1：try{} catch(){}finally{} return;
            显然程序按顺序执行。
* 情况2:try{ return; }catch(){} finally{} return;
          程序执行try块中return之前（包括return语句中的表达式运算）代码；
         再执行finally块，最后执行try中return;
         finally块之后的语句return，因为程序在try中已经return所以不再执行。
* 情况3:try{ } catch(){return;} finally{} return;
         程序先执行try，如果遇到异常执行catch块，
         有异常：则执行catch中return之前（包括return语句中的表达式运算）代码，再执行finally语句中全部代码，
                     最后执行catch块中return. finally之后也就是4处的代码不再执行。
         无异常：执行完try再finally再return.
* 情况4:try{ return; }catch(){} finally{return;}
          程序执行try块中return之前（包括return语句中的表达式运算）代码；
          再执行finally块，因为finally块中有return所以提前退出。
* 情况5:try{} catch(){return;}finally{return;}
          程序执行catch块中return之前（包括return语句中的表达式运算）代码；
          再执行finally块，因为finally块中有return所以提前退出。
* 情况6:try{ return;}catch(){return;} finally{return;}
          程序执行try块中return之前（包括return语句中的表达式运算）代码；
          有异常：执行catch块中return之前（包括return语句中的表达式运算）代码；
                       则再执行finally块，因为finally块中有return所以提前退出。
          无异常：则再执行finally块，因为finally块中有return所以提前退出。

* 最终结论：任何执行try 或者catch中的return语句之前，都会先执行finally语句，如果finally存在的话。
                  如果finally中有return语句，那么程序就return了，所以finally中的return是一定会被return的，
                  编译器把finally中的return实现为一个warning。

### 测试程序

```
    public static void main(String[] args) throws IllegalArgumentException, IllegalAccessException {
        System.out.println(Test.test1()); // 4
        System.out.println(test1_x);      // 9
        System.out.println(Test.test2()); // 9
        System.out.println(test2_x);      // 9

        System.out.println(Test.test3()); // 7
        System.out.println(test3_x);      // 7
    }

    static int test1_x = 1;
    static int test1() {
        try {
            test1_x += 3;
            return test1_x;
        } finally {
            test1_x += 5;
        }
    }
    static int test2_x = 1;
    static int test2() {
        try {
            test2_x += 3;
            return test2_x;
        } finally {
            test2_x += 5;
            return test2_x;
        }
    }


    static Integer test3_x= new Integer(0);
    static Integer test3()  {
        try {
            test3_x += 3;
            return test3_x;
        }finally{
            try{
                Class<Integer> clazz = (Class<Integer>) test3_x.getClass();
                Field[] fields = clazz.getDeclaredFields();
                Field f = null;
                for (int i = 0; i < fields.length; i++) {
                    if ("value".equals(fields[i].getName())){
                        f = fields[i];
                    }
                }
                f.setAccessible(true);
                f.setInt(test3_x, 7);
            }catch (Exception e){
                e.printStackTrace();
            }
        }
    }
```
                  
参考：

[有return的情况下try catch finally的执行顺序（最有说服力的总结） - fery - 博客园](https://www.cnblogs.com/fery/p/4709841.html)

[java return与finally执行顺序测试 - coder_no的博客 - CSDN博客](https://blog.csdn.net/coder_no/article/details/83581442)


