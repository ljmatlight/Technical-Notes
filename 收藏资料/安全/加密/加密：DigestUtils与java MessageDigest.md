1.使用Spring的DigestUtils

[java] view plain copy
public class StringUtilTest {  
  
    static final String TARGET = "changeme";      
      
    /*  
     * 不可逆算法  MD5  
     */    
    @Test    
    public void Md5()    
    {    
        String str = DigestUtils.md5DigestAsHex(TARGET.getBytes());    
        System.out.println("md5Hex:     "+str);    
    }    
}  
        结果为：
              md5Hex:     4cb9c8a8048fd02294477fcb1a41191a
        缺点为：只提供了MD5的加密算法



2.使用commons-codec

[java] view plain copy
public class UtilTest {  
  
    static final String TARGET = "changeme";      
      
    /*  
     * 不可逆算法  MD5  
     */    
    @Test    
    public void Md5()    
    {    
        String str = DigestUtils.md5Hex(TARGET);    
        System.out.println("md5Hex:     "+str);    
    }    
    /*  
     * 不可逆算法  SHA1  
     */    
    @Test    
    public void Sha1()    
    {    
        String str = DigestUtils.shaHex(TARGET);    
        print("shaHex:     "+str);    
        str = DigestUtils.sha256Hex(TARGET);    
        print("sha256Hex:  "+str);    
        str = DigestUtils.sha384Hex(TARGET);    
        print("sha384Hex:  "+str);    
        str = DigestUtils.sha512Hex(TARGET);    
        print("sha512Hex:  "+str);    
    }    
        
       
     /*  
      * 可逆算法  BASE64  
      */  
         
    @Test    
    public void Base64()    
    {    
        //加密    
        byte[] b = Base64.encodeBase64(TARGET.getBytes(), true);    
        String str = new String(b);    
        print("BASE64:     "+str);    
            
        //解密    
        byte[] b1 = Base64.decodeBase64(str);    
        print("解密之后内容为：  "+new String(b1));    
    }    
    public void print(Object obj)    
    {    
        System.out.println(obj);    
    }    
}  

3.java MessageDigest
[java] view plain copy
@Test  
public void test()   
{  
    try   
    {  
        String password = "12345psw";  
          
        //MD5表示加密算法，可以选择其他参数，如SHA-1等  
        MessageDigest digest = MessageDigest.getInstance("MD5");  
        //先调用update，再调动digest  
        digest.update(password.getBytes());  
        byte[] byteResult = digest.digest();  
          
        //因为加密完为字节数组，需要转化为字符串  
        String result = convertbyte2String(byteResult);  
          
        System.out.println(result);  
    }  
    catch (NoSuchAlgorithmException e)   
    {  
        e.printStackTrace();  
    }  
      
}  
  
//将字节数组转化为字符串  
private String convertbyte2String(byte[] byteResult)   
{  
    char[] hexDigits = {'0','1','2','3','4','5','6','7','8','9', 'A','B','C','D','E','F' };  
      
    //4位代表一个16进制，所以长度需要变为原来2倍  
    char[] result = new char[byteResult.length*2];  
      
    int index = 0;  
    for(byte b:byteResult)  
    {  
        //先转换高4位  
        result[index++] = hexDigits[(b>>>4)& 0xf];  
        result[index++] = hexDigits[b& 0xf];  
    }  
    return new String(result);  
}  


原文出自：未知 