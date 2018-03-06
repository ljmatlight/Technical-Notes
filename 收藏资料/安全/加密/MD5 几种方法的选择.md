md5加密实现方法有很多种，也导致很难选择。大概分析下自己了解的一些用法。 

1.sun官方 
sun提供了MessageDigest和BASE64Encoder可以用指定算法加密。 
例： 
Java代码  收藏代码
public static final String EncoderPwdByMd5(String str) throws                    NoSuchAlgorithmException,UnsupportedEncodingException  
    {  
        // 确定计算方法  
        MessageDigest md5 = MessageDigest.getInstance("MD5");  
        BASE64Encoder base64en = new BASE64Encoder();  
        // 加密后的字符串，注意一定要自己指定编码，否则会取系统默认。不同系统会不一致。  
        String newstr = base64en.encode(md5.digest(str.getBytes("utf-8")));  
        return newstr;  
    }  


分析： 
1）BASE64Encoder是不建议使用的，引入有时候也会报错： 
Access restriction: The type BASE64Encoder is not accessible due to restriction on required library C:\Program files\java\jdk1.6\jre\lib\rt.jar 
oracle官方有文档说明(Why Developers Should Not Write Programs That Call 'sun' Packages)，sun.*下面的类不建议使用： 
http://www.oracle.com/technetwork/java/faq-sun-packages-142232.html 

但也有两种规避办法。 
方法一： 
1. Open project properties. 
2. Select Java Build Path node. 
3. Select Libraries tab. 
4. Remove JRE System Library. 
5. Add Library JRE System Library. 

方法二： 
Go to Window-->Preferences-->Java-->Compiler-->Error/Warnings. 
Select Deprecated and Restricted API. Change it to warning. 
Change forbidden and Discouraged Reference and change it to warning. (or as your need.) 

另外： 
使用MessageDigest不使用BASE64Encoder也可以实现md5加密，但要自己实现md5算法， 
比较麻烦。可以参考： 
http://blog.csdn.net/xiao__gui/article/details/8148203http://blog.csdn.net/xiao__gui/article/details/8148203 
http://wenku.baidu.com/link?url=pgf96g_dt2r2vEE88RG7jqMaW3PCSmxL_3sEBwbNb4EzLalQnb-hUsAB1bnqotbAlCDTT60WvFdS0hn9QTeSJAUtahDgpWE9Z_S-yM8Y6-a 

2.sun官方和第三方结合 
也可以使用MessageDigest 加第三方apache commons-codec的支持： 
Java代码  收藏代码
final MessageDigest messageDigest = MessageDigest.getInstance("MD5");  
messageDigest.reset();  
messageDigest.update(string.getBytes(Charset.forName("UTF8")));  
final byte[] resultByte = messageDigest.digest();  
String result = Hex.encodeHexString(resultByte);  


注意： 
以上两种方法都使用了MessageDigest，需要特别强调：MessageDigest线程不安全。 The MessageDigest classes are NOT thread safe. If they're going to be used by different threads, just create a new one, instead of trying to reuse them. 

3.使用第三方工具包 
很多第三方工具都提供了md5，sha等加密方法。apache,google等都提供了工具包。 

3.1 apache的commons-codec 
1）maven配置（现在的版本有很多，选择自己需要的）： 
        <dependency> 
            <groupId>commons-codec</groupId> 
            <artifactId>commons-codec</artifactId> 
            <version>1.6</version> 
        </dependency> 
2）引入包后： 
Java代码  收藏代码
public static String encodeMD5Hex(String data)  
    {  
        return DigestUtils.md5Hex(data);  
    }  

并且该方法是线程安全的。 

3.2 google的guava 
Apache Common是一个时间比较久的框架了，Google针对基础框架退出了自己的类库，并且开源出来(http://code.google.com/p/guava-libraries/)，名为“Guava”。它在部分功能上其实是ApacheCommon的一个子集，但在性能上做了很多优化，并且针对并发和大规模系统开发做了很多新的策略(如CopyOnWrite、Immutable、SkipList)等。虽然有些类和java.util.concurrent有些重叠，但是在一般环境下都可以替代。 

md5示例： 
Java代码  收藏代码
Hasher hasher = Hashing.md5().newHasher();  
hasher.putString("my string");  
byte[] md5 = hasher.hash().asBytes();  

既方便又安全。 
此外，其他组织或公司也有对外提供的工具类，额。。还不清楚。 


原文出自： http://zoroeye.iteye.com/blog/2026984