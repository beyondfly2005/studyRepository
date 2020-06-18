## 八 接口安全机制

### 1  安全机制的设计方案

#### 1-1 单项加密

在理论上，从明文加密到密文后，不可反向解密。

可以从迭代和加盐的方式尽可能保证加密数据不可反向解密。

传递敏感数据时使用的，如密码

在金融相关交易中，用户密码是敏感数据，所有的金融相关的应用中，客户端都有一个独立的密码输入控件，这个控件就是做单项加密的

使用单项加密的时候，传递的数据只有密文，没有明文，也没有秘钥。

#### 1-2 双向加密

时刻实现加密和解密双向运算的算法，需要通过秘钥实现	解密计算

秘钥种类：公钥 、私钥

公钥：可以对外开放的，就是可以在网络中传递的

私钥：必须保密，绝对不会对外暴露的。

在传递安全数据的时候使用，所谓安全数据就是不可篡改的数据，如金融交易中的收款人卡号，转账的金额，货币的种类等。

使用双向加密的是，传递的数据有明文 密文 公钥。

##### 1-2-1对称加密

只有一个秘钥，就是公钥。

##### 1-2-2非对称加密

有两个秘钥，公钥和私钥

### 2 DES加密

```html
mode-ecb-min.js
tripledes.js

<script>
function uuid(){
    var hexDigits = "0123456789abcdef";
    for (var i = 0; i < 36; i++) {
        s[i] = hexDigits.substr(Math.floor(Math.random() * 0x10), 1);
    }
    s[14] = "4";  // bits 12-15 of the time_hi_and_version field to 0010
    s[19] = hexDigits.substr((s[19] & 0x3) | 0x8, 1);  // bits 6-7 of the clock_seq_hi_and_reserved to 01
    s[8] = s[13] = s[18] = s[23] = "-";
 
    var uuid = s.join("");
    return uuid;
}

function encryptByDES(message, key){
	var keyHex = CryptoJs.enc.Utf8.parse(key);
	var encrypted -CryptoJs.DES.encrypt(message,keyHex,{
        mode:CryptoJS.mod.ECB,		//加密模式
        padding:CryptJS.pad.Pkcs7 	//加密的padding
    })
}
function decryptByDES(ciphertext,key){
	var keyHex = CryptoJS,DES.decrypt({
        
    });
}
</script>
```

```java
//
DescCrypt.encode(resKey, message);

public class DesCrypt{
    private static final String KEY="";
    private static final CODE_TYPE="UTF-8";
    
    public static Stromg encode (String ){
        if(null==key){
            key=KEY;
        }
        SecureRandom random = new SecureRandom();
        DESKeySpec desKey = new DESKeySpec(key.getBytes(CODE_TYPE));
        SecretKeyFactory keyFactory 
        Cipher cipher =cipher.getInstance("DES");
        byte[] temp=Base64.encodeBase64(cipher.doFinal(datasource.getBytes()));
        //不要使用new string不定长
        //Base64这是编码解密 保证长度可控
        return IOUtils.toStirng();
    }
    
    //解密
    public static String decode(String key,String src) throws Exception {
        if(null == key){
            
        }
    }
}
```



### 3 AES加密

```bash
aes.min.js
function uuid(){

}

```

