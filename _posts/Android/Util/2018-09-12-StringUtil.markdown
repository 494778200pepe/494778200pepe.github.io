---
layout: post
title:  "StringUtil"
date:   2018-09-12 09:01:00 +0800
categories: Android
tags: Util
author: pepe
description: 『 StringUtil 』
---

```
import android.text.TextUtils;

/**
 * @author wang
 * @date 2018/9/12.
 */

public class StringUtil {

    /**
     * 16进制的字符串表示转成字节数组
     * @param hexString 16进制格式的字符串
     * @return 转换后的字节数组
     **/
    public static byte[] toByteArray(String hexString) {
        if (TextUtils.isEmpty(hexString)){
            throw new IllegalArgumentException("this hexString must not be empty");
        }

        hexString = hexString.toLowerCase();
        final byte[] byteArray = new byte[hexString.length() / 2];
        int k = 0;
        for (int i = 0; i < byteArray.length; i++) {//因为是16进制，最多只会占用4位，转换成字节需要两个16进制的字符，高位在先
            byte high = (byte) (Character.digit(hexString.charAt(k), 16) & 0xff);
            byte low = (byte) (Character.digit(hexString.charAt(k + 1), 16) & 0xff);
            byteArray[i] = (byte) (high << 4 | low);
            k += 2;
        }
        return byteArray;
    }

    /**
     * 字节数组转成16进制表示格式的字符串
     * @param byteArray 需要转换的字节数组
     * @return 16进制表示格式的字符串
     **/
    public static String toHexString(byte[] byteArray) {
        if (byteArray == null || byteArray.length < 1)
            throw new IllegalArgumentException("this byteArray must not be null or empty");

        final StringBuilder hexString = new StringBuilder();
        for (int i = 0; i < byteArray.length; i++) {
            if ((byteArray[i] & 0xff) < 0x10)//0~F前面不零
                hexString.append("0");
            hexString.append(Integer.toHexString(0xFF & byteArray[i]));
        }
        return hexString.toString().toLowerCase();
    }


    /**
     * 16进制的字符串表示转成short数组
     * @param hexString 16进制格式的字符串
     * @return 转换后的short数组
     **/
    public static short[] toShortArray(String hexString) {
        if (TextUtils.isEmpty(hexString)) {
            throw new IllegalArgumentException("this hexString must not be empty");
        }

		hexString = hexString.toLowerCase();
		final short[] shortArray = new short[hexString.length() / 4];
		int k = 0;
		for (int i = 0; i < shortArray.length; i++) {
			short one = (short) (Character.digit(hexString.charAt(k), 16) & 0xff);
			short two = (short) (Character.digit(hexString.charAt(k + 1), 16) & 0xff);
			short three = (short) (Character.digit(hexString.charAt(k + 2), 16) & 0xff);
			short four = (short) (Character.digit(hexString.charAt(k + 3), 16) & 0xff);

            shortArray[i] = (short) (one << 12 | two << 8 | three << 4 | four);
			k += 4;
		}
        return shortArray;
    }

    /**
     * short数组转成16进制表示格式的字符串
     * @param shortArray 需要转换的short数组
     * @return 16进制表示格式的字符串
     **/
    public static String toHexString(short[] shortArray) {
        if (shortArray == null || shortArray.length < 1) {
            throw new IllegalArgumentException("this shortArray must not be null or empty");
        }

		final StringBuilder hexString = new StringBuilder();
		for (int i = 0; i < shortArray.length; i++) {
			int s = shortArray[i] & 0xffff;
			if (s < 0x10) {//0~F前面不零
				hexString.append("0");
				hexString.append("0");
				hexString.append("0");
			} else if (s < 0x100) {
				hexString.append("0");
				hexString.append("0");
			} else if (s < 0x1000) {
				hexString.append("0");
			}
			hexString.append(Integer.toHexString(0xFFFF & shortArray[i]));
		}
        return hexString.toString().toLowerCase();
    }
}
```




