> 视频地址 https://www.bilibili.com/video/BV1VJ411g7Zq?p=86

### SpringBoot 整合JavaMail



#### 1 使用场景

- 注册验证，激活账号
- 通过注册邮箱找回密码
- 系统监控告警
- 网站营销



#### 2 邮件发送协议

- 邮件传输协议：SMTP协议和POP3协议

  - POP3是Post Office Protocol3的简称，即邮件协议的第三个版本，它规定怎样将个人计算机连接到Internet的邮件服务器和下载电子邮件的电子协议。它是因特网电子邮件的第一个离线协议标准。POP3允许用户从服务器上把邮件存储到本地主机（即自己的计算机）上,同时删除保存在邮件服务器上的邮件，而POP3服务器则是遵循POP3协议的接收邮件服务器，用来接收电子邮件的。

  - SMTP(Simple Mail Transfer Protocol)即简单邮件传输协议,它是一组用于由源地址到目的地址传送邮件的规则,由它来控制信件的中转方式。SMTP协议属于TCP／IP协议族,它帮助每台计算机在发送或中转信件时找到下一个目的地。通过SMTP协议所指定的服务器,我们就可以把E－mail寄到收信人的服务器上了,整个过程只要几分钟。SMTP服务器则是遵循SMTP协议的发送邮件服务器，用来发送或中转你发出的电子邮件。 
  - SMTP认证，简单的说就是要求必须在提供了账户名和密码之后才可以登录到SMTP服务器，这就使得那些垃圾邮件的散播者无可乘之机。
  - 证件SMTP认证的目的是为了使用户避免受到垃圾邮件的侵扰。

- IMAP协议

  IMAP协议全称是nternet Mail Access Protocol,交互式邮件存取协议，它是跟POP3类似的邮件访问协议之一，不同的是，抬起了IMAP后，您在电子邮件客户端收取的邮件仍保留在服务器上，同时在客户端上的操作都会反馈到服务器上，如：删除邮件，标记已读等，服务器上的邮件也会做相应的动作，所以无论从浏览器登录邮箱或者从客户端软件登录邮箱，看到的邮件状态是一致的。

- 发送邮件：SMTP

- 接收邮件：POP3/IMAP

- JavaMail



#### 3 配置环境

- 引入依赖

<<<<<<< HEAD
  ```
  <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-mail</artifactId>
          </dependency>
  ```

  

- 配置服务器信息

- 
=======
```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-mail</artifactId>
    </dependency>
```



- 配置服务器信息

```yaml
spring:
  mail: 
    host: smtp.163.com
    username: xx@163.com
    password:
    default-encoding: UTF-8
#自定义邮件发送者    
mail:
  fromAddr: 498601485@qq.com
  
```



#### 4 发送普通邮件

```java
@Service
private class MailServiceImpl implements IMailService{

	@Autowired
	private JavaMailSender mailSender;
	
	@Value("${mail.fromAddr}")
	private from;
	
	@Override
	public void sendSimpleMail(String to,String subject, String content){
		SimpleMailMessage simpleMessage = new SimpleMailMessage();
        simpleMessage.setFrom(from);
        simpleMessage.setTo(to);
        simpleMessage.setSubject(subject);
		simpleMessage.setContent(content);
        mailSender.send(simpleMessage);
	}
}
```



#### 5 发送HTML格式的邮件

```java
@Autowired
private JavaMailSender javaMailSender;

@Override
public void sendHtmleMail(String to,String subject, String content) throws MessagingException {
    MimeMessage mimeMessage = javaMailSender.createMimeMessage();
    MimeMessageHelper helper = new MimeMessageHelper(mimeMessage,true);
    helper.setFrom(from);
    helper.setTo(to);
    helper.setSubject(subject);
	helper.setContent(content,true);
    javaMailSender.send(mimeMessage);
}
```

#### 6 发送带附加的邮件

```

```

#### 7 邮件模板

```

```

>>>>>>> 6a5355b82ec202b1389aa636198bb9d48dc1d0c2
