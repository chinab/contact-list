获取用户的msn和邮箱联系人列表，支持的邮箱包括hotmail, gmail, yahoo, sohu, sina, 163, 126, tom, yeah, 189和139。

contactlist项目首页和jar包下载在http://code.google.com/p/contact-list/

contactlist项目源代码在http://github.com/flyerhzm/contactlist, google code svn不再更新

# Service: #

contactlist提供web api调用，必须使用HTTP POST请求，接口如下：
```
url: https://123.183.209.87:8443/ContactListService/contacts
parameters: account=xxx@gmail.com&password=xxxx&type=gmail
```

返回json格式如下：

正确的结果：
```
{'contacts': [{'username': 'yyy', 'email': 'yyy@gmail.com'}, {'username': 'zzz', 'email': 'zzz@hotmail.com'}]}
```

错误的结果：
```
{'error': 'hotmail protocol changed'}
```

ruby客户端: http://github.com/flyerhzm/contactlist-client

web客户端： http://contactlist.heroku.com

# Example: #
```
try {
    ContactsImporter importer = ContactsImporterFactory.getHotmailContacts(username, password);
    List<Contact> contacts = importer.getContacts();
    for (Contact contact : contacts) {
        System.out.println(contact.getUsername() + ": " + contact.getEmail());
    }
} catch (ContactsException ex) {
    ex.printStackTrace();
}
```


# Encoding: #
项目的输出统一为UTF-8。
对于运行在locale为UTF-8环境（如：Ubuntu, Mac OS X）下的程序，无需任何调整。
对于运行在locale为GBK环境（如：Windows XP）下的程序，需要手动调整编码：
```
try {
    ContactsImporter importer = ContactsImporterFactory.getHotmailContacts(username, password);
    List<Contact> contacts = importer.getContacts();
    for (Contact contact : contacts) {
        String username = new String(contact.getUsername().getBytes("UTF-8"), "GBK");
        System.out.println(username + ": " + contact.getEmail());
    }
} catch (ContactsException ex) {
    ex.printStackTrace();
}
```
另外，在windows下面需要把项目的编码设置为UTF-8，并且确保java文件的编译是用UTF-8的

# Project Introduction: #
contact-list类库依赖包之commons-httpclient -- http://www.huangzhimin.com/entries/142-contact-list-library-dependency-of-commons-httpclient

contact-list类库依赖包之msnmlib -- http://www.huangzhimin.com/entries/147-contact-list-library-dependency-of-msnmlib

contact-list类库依赖包之json -- http://www.huangzhimin.com/entries/158-contact-list-of-the-json-library-dependency

contact-list类库依赖包之gdata -- http://www.huangzhimin.com/entries/162-contact-list-of-the-gdata-library-dependency

持续更新中...

# How to run unit test: #
出于安全的考虑，没有把单元测试中的邮箱配置文件和msn配置文件放到svn上，如果需要运行mvn test的话，可以按以下步骤：

1. 新建src/test/resources/email.properties文件，形式如下：
```
gmail.username = xxx@gmail.com
gmail.password = yyy

hotmail.username = xxx@hotmail.com
hotmail.password = yyy

livecn.username = xxx@live.cn
livecn.password = yyy

onesixthree.username = xxx@163.com
onesixthree.password = yyy

onetwosix.username = xxx@126.com
onetwosix.password = yyy

sina.username = xxx@sina.com
sina.password = yyy

sohu.username = xxx@sohu.com
sohu.password = yyy

tom.username = xxx@tom.com
tom.password = yyy

yahoo.username = xxx@yahoo.com
yahoo.password = yyy

yahoocn.username = xxx@yahoo.cn
yahoocn.password = yyy

yahoocomcn.username = xxx@yahoo.com.cn
yahoocomcn.password = yyy

yeah.username = xxx@yeah.net
yeah.password = yyy

oneeightnine.username = xxx@189.cn
oneeightnine.password = yyy

onethreenine.username = xxx@139.com
oneeightnine.password = yyy
```
2. 新建src/test/resources/msn.properties文件，形式如下：
```
username = xxx@live.cn
password = yyy
```
3. 在命令行执行mvn test


# Change Log: #
  * 1.1:
> > First public release

  * 1.2:
> > Fix issue that hotmail can only get first page contact list

  * 1.3:
> > Fix issue for some special sohu mail account

  * 1.4:
> > Fix issue that 163 mail can only get other group contacts

  * 1.5:
> > Fix issue that 126 and yeah mail can only get other group contacts

  * 1.6:
> > Add 189 and 139 mail support

  * 1.6.1:
> > Meet another style 189 mail and add 139 mail importer to ContactsImporterFactory

  * 1.7.0:
> > Get google contacts by gdata api

  * 1.8.0:
> > Get hotmail contacts by windows live contacts api

  * 1.9.0:
> > Better parser for yahoo and tom mail

  * 1.10.0:
> > Use passportName as email for hotmail

  * 1.11.0:
> > Fix 139 for web change and fix 126 and yeah mail when username is empty

  * 1.12.0:
> > Get msn contacts by window live contacts api



# Other: #
需要留言的话，可以上http://github.com/flyerhzm/contactlist/issues，我会尽快回复的

由于这个类库的原理是使用抓取网页来分析联系人列表的，所以会因为邮箱网页的改版而无法正确获取联系人列表。
如果大家在使用的时候发现有邮箱不能获得联系人列表，希望先把类库log4j的level设置为debug，把调试信息和错误信息一起发送给我flyerhzm@gmail.com，我会尽快解决问题的，谢谢！