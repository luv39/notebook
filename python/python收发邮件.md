# python收发邮件

[TOC]

## 使用email模块构建邮件

```python
from email.mime.text import MIMEText
from email.header import Header
 
msg = MIMEText('邮件正文', 'text', 'utf-8')
msg['Subject'] = Header('标题')
msg['From'] = from_addr_str
msg['To'] = to_addr_str
```

## 使用smtplib模块发送邮件

* 使用SMTP的基本步骤
    1. 连接服务器
    2. 登陆
    3. 发送服务请求
    4. 退出

```python
import smtplib
 
# 使用非SSL协议端口登陆
smtp = smtplib.SMTP()
smtp.connect(mail_smtpserver, 25)
smtp.login(mail_username, mail_password)
smtp.sendmail(mail_from, mail_to, msg.as_string())
smtp.quit()
 
# 使用SSL协议端口登陆
smtp = smtplib.SMTP_SSL(mail_smtpserver, 465)
smtp.login(mail_username, mail_password)
smtp.sendmail(mail_from, mail_to, msg.as_string())
smtp.quit()
```

* 邮件服务器和端口

| 邮箱 |  SMTP服务器  | SSL协议端口 | 非SSL协议端口 |
| :--: | :----------: | :---------: | :-----------: |
| 163  | smtp.163.com | 456或者994  |      25       |
|  qq  | smtp.11.com  | 456或者587  |      25       |

## 使用POP3接收邮件

* python的poplib模块支持POP3，基本步骤：
    1. 连接到服务器
    2. 登陆
    3. 发出服务请求
    4. 退出

* poplib的常用方法：

|       方法       |                             描述                             |
| :--------------: | :----------------------------------------------------------: |
| POP3_SSL(server) |  实例化POP3对象，server是pop服务器地址，使用SSL协议端口连接  |
|  user(username)  |            发送用户名到服务器，等待服务器返回信息            |
| pass_(password)  |                             密码                             |
|      stat()      |     返回邮箱的状态，返回2元组(消息的数量，消息的总字节)      |
|  list([msgnum])  | stat()的扩展，返回一个3元组(返回信息, 消息列表, 消息的大小)，如果指定msgnum，就只返回指定消息的数据 |
|   retr(msgnum)   | 获取详细msgnum，设置为已读，返回3元组(返回信息, 消息msgnum的所以内容, 消息的字节数)，如果指定msgnum，就只返回指定消息的数据 |
|   dele(msgnum)   |                     将指定消息标记为删除                     |
|      quit()      |           登出，保存修改，解锁邮箱，结束连接，退出           |

```python
from poplib import POP3_SSL
 
p = POP3_SSL('pop.163.com')
p.user('xxxxxxx@163.com')
p.pass_('xxxxxxxx')
 
p.stat()
pass
p.quit()
```

* POP服务器

| 邮箱 |  POP服务器  |
| :--: | :---------: |
| 163  | pop.163.com |

## 使用IMAP接收邮件

* python中的imaplib包支持IMAP4

常用方法：

|       方法        |                             描述                             |
| :---------------: | :----------------------------------------------------------: |
|   IMAP4(server)   |                     与IMAP服务器建立连接                     |
| login(user, pass) |                         用户密码登录                         |
|      list()       |           查看所有的文件夹(IMAP可以支持创建文件夹)           |
|     select()      |                   选择文件夹默认是"INBOX"                    |
|     search()      | 三个参数，第一的是CHARSET,通常为None(ASCII),第二个参数不知到是干什么官方没解释 |

```python
import getpass, imaplib
 
M = imaplib.IMAP4()
M.login(getpass.getuser(), getpass.getpass())
M.select()
typ, data = M.search(None, 'ALL')
for num in data[0].split():
    typ, data = M.fetch(num, '(RFC822)')
    print 'Message %s\n%s\n' % (num, data[0][1])
M.close()
M.logout()
```

* IMAP服务器

| 邮箱 |  IMAP服务器  |
| :--: | :----------: |
| 163  | imap.163.com |