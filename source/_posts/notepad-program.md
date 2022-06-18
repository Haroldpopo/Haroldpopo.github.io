---
title: C#编写一个较完整的记事本程序
date:  2020-02-17 18:41:00
---

:::primary

本文章在我的CSDN和博客园账号都有发布，这里做了一点修改，完整代码请下载项目源码查阅或移步至博客园中阅读，附博客园本文地址：[https://www.cnblogs.com/Harold-popo/p/12436682.html](https://www.cnblogs.com/Harold-popo/p/12436682.html)

CSDN博客账号已不更新，这里也附上地址：[https://blog.csdn.net/UFO_Harold/article/details/104349663](https://blog.csdn.net/UFO_Harold/article/details/104349663)

:::

# 开发环境

Visual Studio 2019

- 至少需安装 **.NET桌面开发**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228133154627.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VGT19IYXJvbGQ=,size_16,color_FFFFFF,t_70)

# 创建项目并配置

## 创建窗体文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228133341997.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VGT19IYXJvbGQ=,size_16,color_FFFFFF,t_70)

## 配置项目名称及框架

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228133429889.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VGT19IYXJvbGQ=,size_16,color_FFFFFF,t_70)

## 设计界面

创建窗体文件，将控件摆放位置如下，参考系统自带的记事本程序
![窗体控件分布](https://img-blog.csdnimg.cn/20200217141620298.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VGT19IYXJvbGQ=,size_16,color_FFFFFF,t_70)

## 窗体添加的控件和组件如下

- 控件及组件在工具箱查找

![窗体所需添加的控件和组件](https://img-blog.csdnimg.cn/20200216215506680.png)

## 窗体属性

![窗体属性](https://img-blog.csdnimg.cn/20200217133721429.png)

## 快捷键设置

- 杂项 --> ShortcutKeys

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200217143656219.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VGT19IYXJvbGQ=,size_16,color_FFFFFF,t_70)

## 程序属性

项目属性如下图，在创建项目时就已定好了框架，如果在另一台主机上的框架版本比目前项目框架版本低的话，则运行不起来

- 文章末尾有整个程序的压缩包链接可下载，如需直接运行则需下载对应的`.NET Framework 4.7.2`框架

![项目属性](https://img-blog.csdnimg.cn/20200217140540435.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VGT19IYXJvbGQ=,size_16,color_FFFFFF,t_70)

程序图标可在此设置，生成程序后的图标如下图，此文件夹下的程序文件可在第二台主机上直接运行（项目\bin\Debug目录下就是生成程序文件的存放位置，双击程序文件即可运行）

![项目生成文件的路径](https://img-blog.csdnimg.cn/20200217141129759.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VGT19IYXJvbGQ=,size_16,color_FFFFFF,t_70)

# 代码演示

## 引用的命名空间

:::warning

注释部分需自行添加

:::

```csharp 命名空间的引用
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO; // 提供了关于文件、数据流的读取和写入操作
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Diagnostics; // 提供了用于与事件日志、性能计数器和系统进程进行交互的类
```

## 主要功能

### 新建文件

```csharp 新建文件
private void 新建NToolStripMenuItem_Click(object sender, EventArgs e) {
    if (txtBox.Modified == true) {
        DialogResult dr = MessageBox.Show("文件发生变化，是否更改保存？", "注意", MessageBoxButtons.YesNoCancel);
        
        if (dr == DialogResult.Yes) {
            保存SToolStripMenuItem_Click(sender, e);
            return;
        } else if (dr == DialogResult.Cancel) {
            return;
        }
        
        txtBox.Clear();
        this.Text = "NewNotepad";
    }else{
        txtBox.Clear();
        this.Text = "NewNotepad";
    }
}
```

### 打开

```csharp 打开
private void 打开ToolStripMenuItem_Click(object sender, EventArgs e) {
    if (openFileDialog.ShowDialog() == DialogResult.OK) {
        filename = openFileDialog.FileName;
        OpenFile();
    }
}

protected void OpenFile() {
    try {
        txtBox.Clear();
        txtBox.Text = File.ReadAllText(filename);
    } catch {
        MessageBox.Show("Error!");
    }
}
```

### 保存

```csharp 保存
private void 保存SToolStripMenuItem_Click(object sender, EventArgs e) {
    try {
        StreamWriter sw = File.AppendText(Application.ExecutablePath);
        sw.Write(txtBox.Text);
        sw.Dispose();
    } catch {
        SaveFileDialog sf = new SaveFileDialog();
        sf.DefaultExt = "*.txt";
        sf.Filter = "文本文档(.txt)|*.txt";
        if (sf.ShowDialog() == DialogResult.OK) {
            StreamWriter sw = File.AppendText(sf.FileName);
            sw.Write(txtBox.Text);
            sw.Dispose();
        }
    }
}
```

### 另存为

```csharp 另存为
private void 另存为ToolStripMenuItem_Click(object sender, EventArgs e) {
    string name;
    // SaveFileDialog类
    SaveFileDialog save = new SaveFileDialog();
    // 过滤器
    save.Filter = "*.txt|*.TXT|(*.*)|*.*";
    // 显示
    if (save.ShowDialog() == DialogResult.OK) {
        name = save.FileName;
        FileInfo info = new FileInfo(name);
        // info.Delete();
        StreamWriter writer = info.CreateText();
        writer.Write(txtBox.Text);
        writer.Close();
    }
}
```

### 打印

```csharp 打印
private void 打印PToolStripMenuItem_Click(object sender, EventArgs e) {
    // 显示允许用户选择打印机的选项及其它打印选项的对话框
    this.printDialog.Document = this.printDocument;
    this.printDialog.PrinterSettings = this.pageSetupDialog.PrinterSettings;
    // 向打印机发送打印指令
    if (this.printDialog.ShowDialog() == DialogResult.OK) {
        try {
            this.printDocument.Print();
        } catch (Exception ex) {
            MessageBox.Show(ex.Message, "错误信息！", MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
}
```

### 编辑

:::default

根据 是否输入内容 控制 是否启用功能

:::

```csharp 编辑
private void 编辑ToolStripMenuItem_Click(object sender, EventArgs e) {
    剪切ToolStripMenuItem.Enabled = txtBox.Modified;
    if (txtBox.SelectedText == "") {
        剪切ToolStripMenuItem.Enabled = false;
        复制ToolStripMenuItem.Enabled = false;
        删除ToolStripMenuItem.Enabled = false;
    } else {
        剪切ToolStripMenuItem.Enabled = true;
        复制ToolStripMenuItem.Enabled = true;
        删除ToolStripMenuItem.Enabled = true;
    }
    if (txtBox.Text == "") {
        查找ToolStripMenuItem.Enabled = false;
        查找下一个ToolStripMenuItem.Enabled = false;
        查找上一个ToolStripMenuItem.Enabled = false;
        替换ToolStripMenuItem.Enabled = false;
    } else {
        查找ToolStripMenuItem.Enabled = true;
        查找下一个ToolStripMenuItem.Enabled = true;
        查找上一个ToolStripMenuItem.Enabled = true;
        替换ToolStripMenuItem.Enabled = true;
    }
    if (Clipboard.GetText() == "")
        粘贴ToolStripMenuItem.Enabled = false;
    else
        粘贴ToolStripMenuItem.Enabled = true;
}
```

### 查找

:::warning

查找功能不够完善，混用查找上一项和查找下一项效果不理想

:::

```csharp 查找
private void 查找ToolStripMenuItem_Click(object sender, EventArgs e) {
    //显示查找对话框
    txtInput.Size = new Size(190, 33);
    txtInput.Location = new Point(70, 15);
    txtInput.Multiline = true;

    FindNext.Click += new EventHandler(Direction_Click);
    //FindNext.Click += new EventHandler(Visible_Click);

    Cancel.Click += new EventHandler(Cancel_Click);

    FindForm.Controls.Add(lblSearch);
    FindForm.Controls.Add(lblDirection);
    FindForm.Controls.Add(txtInput);
    FindForm.Controls.Add(down);
    FindForm.Controls.Add(upward);
    FindForm.Controls.Add(FindNext);
    FindForm.Controls.Add(Cancel);
    FindForm.Top = this.Top + 50;
    FindForm.Left = this.Left + 50;
    FindForm.Height = 120;
    FindForm.Width = 380;
    FindForm.StartPosition = FormStartPosition.CenterParent;
    FindForm.ShowDialog();
}

private void Cancel_Click(object sender, EventArgs e) {
    //关闭对话框
    FindForm.Close();
    ReplaceForm.Close();
}

private void Direction_Click(object sender, EventArgs e) {
    //选择字符查找方向
    if (down.Checked == true) {
        Find_Click(sender, e);
    } else if (upward.Checked == true) {
        FindLast_Click(sender, e);
    }
}

int nextPosition, firstPosition;
string word;
Boolean IF = false;

private void Find_Click(object sender, EventArgs e) {
    txtBox.Focus();
    FindWords(txtInput.Text);
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228135353669.jpg)

### 查找下一项 

```csharp 查找下一项 
private void FindWords(string words) {
    //向下查找字符
    if (nextPosition >= txtBox.Text.Length)
        nextPosition = 0;
    firstPosition = txtBox.Text.IndexOf(words, nextPosition);
    if (firstPosition == -1)
        nextPosition = 0;
    else {
        txtBox.Select(firstPosition, words.Length);
        nextPosition = firstPosition + 1;
    }
    word = words;
    IF = true;
}
```

### 查找上一项

```csharp 查找上一项
private void FindWordsLast(string words) {
    //向上查找字符
    if (IF == false)
        nextPosition = txtBox.Text.Length;
    if (nextPosition < 0)
        nextPosition = txtBox.Text.Length;

    firstPosition = txtBox.Text.LastIndexOf(words, nextPosition);

    if (firstPosition == -1)
        nextPosition = txtBox.Text.Length;
    else {
        txtBox.Select(firstPosition, words.Length);
        nextPosition = firstPosition - 1;
    }
    word = words;
    IF = true;
}

private void 查找上一个ToolStripMenuItem_Click(object sender, EventArgs e) {
    //查找上一项，如果未查找过，则显示查找对话框
    upward.Checked = true;
    down.Checked = false;
    try {
        FindWordsLast(word);
    } catch {
        查找ToolStripMenuItem_Click(sender, e);
    }
}
```

### 替换

```csharp 替换
private void 替换ToolStripMenuItem_Click(object sender, EventArgs e) {
    txtInput.Size = new Size(190, 30);
    txtInput.Location = new Point(70, 12);
    txtInput.Multiline = true;

    txtInputReplace.Size = new Size(190, 30);
    txtInputReplace.Location = new Point(70, 47);
    txtInputReplace.Multiline = true;

    Button Replace = new Button {
        Name = "btnReplace",
        Text = "替换",
        Size = new Size(80, 25),
        Location = new Point(265, 15)
    };
    Replace.Click += new EventHandler(Replace_Click);
    Cancel.Click += new EventHandler(Cancel_Click);

    ReplaceForm.Controls.Add(lblSearch);
    ReplaceForm.Controls.Add(LblReplace);
    ReplaceForm.Controls.Add(txtInput);
    ReplaceForm.Controls.Add(txtInputReplace);
    ReplaceForm.Controls.Add(Replace);
    ReplaceForm.Controls.Add(Cancel);
    ReplaceForm.Top = this.Top + 50;
    ReplaceForm.Left = this.Left + 50;
    ReplaceForm.Height = 140;
    ReplaceForm.Width = 380;
    ReplaceForm.StartPosition = FormStartPosition.CenterParent;
    ReplaceForm.ShowDialog();
}

private void Replace_Click(object sender, EventArgs e) {
    txtBox.Text = txtBox.Text.Replace(txtInput.Text, txtInputReplace.Text);
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228135328450.jpg)

### 字体选择

:::default

直接调用控件即可

:::

```csharp 字体选择
private void 字体ToolStripMenuItem_Click(object sender, EventArgs e) {
    //提示用户从本地计算机安装的字体中选择字体字号
    FontDialog fontDialog = new FontDialog();
    if (fontDialog.ShowDialog() == DialogResult.OK) {
        txtBox.Font = fontDialog.Font;
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228135229675.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VGT19IYXJvbGQ=,size_16,color_FFFFFF,t_70)

### 关于记事本

:::default

新建一个窗口，根据自己的喜好添加标签及摆放位置

:::

```csharp 关于记事本
private void 关于记事本ToolStripMenuItem_Click(object sender, EventArgs e) {
    //关于记事本说明
    Label lblTitle = new Label() {
        Text = "多功能记事本",
        Size = new Size(150, 25),
        Location = new Point(100, 50)
    };
    Label lblEdition = new Label() {
        Text = "版本号：个性测试版",
        Size = new Size(150, 25),
        Location = new Point(85, 100)
    };
    Label lblMail = new Label() {
        Text = "E-Mail：",
        Size = new Size(55, 25),
        Location = new Point(30, 180)
    };
    LinkLabel llblMail = new LinkLabel() {
        Text = "2417525822@qq.com",
        Size = new Size(110, 25),
        Location = new Point(85, 180)
    };
    Label lblCNDS = new Label() {
        Text = "CNDS博客：",
        Size = new Size(65, 25),
        Location = new Point(20, 220)
    };
    LinkLabel llblCNDS = new LinkLabel() {
        Text = "https://blog.csdn.net/UFO_Harold",
        Size = new Size(200, 25),
        Location = new Point(85, 220)
    };
    Form about = new Form {
        Text = "关于记事本",
        FormBorderStyle = FormBorderStyle.FixedSingle,
        MaximizeBox = false
    };

    llblCNDS.Click += new EventHandler(LlblCNDS_Click);
    about.Controls.Add(lblTitle);
    about.Controls.Add(lblEdition);
    about.Controls.Add(lblMail);
    about.Controls.Add(llblMail);
    about.Controls.Add(lblCNDS);
    about.Controls.Add(llblCNDS);
    about.Top = this.Top + this.Height / 2 - about.Height / 2;
    about.Left = this.Left + this.Width / 2 - about.Width / 2;
    about.StartPosition = FormStartPosition.CenterParent;
    about.ShowDialog();
}
```

[:heavy_check_mark:成功效果图]{.label .success}

![程序的关于记事本功能展示](https://img-blog.csdnimg.cn/20200217134445410.jpg#pic_center)

## 运行结果

![win10环境下运行程序的结果](https://img-blog.csdnimg.cn/20200217134306736.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VGT19IYXJvbGQ=,size_16,color_FFFFFF,t_70#pic_center)

:::warning

注意

:::

1. 控件请自行改名，也可使用默认控件名，此次程序的控件均已自定义名称，然后再双击控件便会自动创建控件的事件函数并跳到代码页，全数copy代码到自己新建的程序可能运行不起来，因为控件的事件需要双击控件才跳转到事件函数，事件方法前出现引用不是为 0 即生效；
2. 查找上一项下一项功能混用时会有一些bug，达不到预期效果，但能运行，不会报错，一点逻辑上的问题，目前没有想到解决方法，大家可自行深入摸索，如有可以改进的地方可联系作者加以改进；

:::danger

项目源码仅供学习交流使用，如需项目运行请先安装`.NET Framework 4.7.2`框架，且图标可能因文件路径不同而无法显示，修改文件路径即可

:::

- 蓝奏云：[https://wwi.lanzouw.com/i9r643e](https://wwi.lanzouw.com/i9r643e)（链接已修正）
- 百度网盘：[https://pan.baidu.com/s/1BagLHS9bOG2jvaOcHgeUgA](https://pan.baidu.com/s/1BagLHS9bOG2jvaOcHgeUgA) 提取码：y639
- Github：[https://github.com/Haroldpopo/Notepad](https://github.com/Haroldpopo/Notepad)（链接已修正）

- ***状态栏图标设置***

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228132709890.jpg)

- ***项目文件目录***

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228140257537.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VGT19IYXJvbGQ=,size_16,color_FFFFFF,t_70)

 :::success

项目完成

:::