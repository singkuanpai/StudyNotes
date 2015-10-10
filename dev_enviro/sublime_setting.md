sublime

### 1.初步安装包管理器 
通过快捷键 ctrl+` 或者 View > Show Console 菜单打开控制台
粘贴对应版本的代码后回车安装
适用于 Sublime Text 3：
```
import urllib.request,os;  
pf = 'Package Control.sublime-package';  
ipp = sublime.installed_packages_path();  
urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) );  
open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```


适用于 Sublime Text 2：
```
import  urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp)ifnotos.path.exists(ipp)elseNone;urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read());print('Please restart Sublime Text to finish installation')
```

### 2.包安装和移除
安装：热键CTRL+SHIFT+P调出Package Control ，查找install package ，输入package  
移除：热键CTRL+SHIFT+P调出Package Control ，查找remove package ，输入package  
查看：按ctrl+shift+p，输入package，选择list packages，就看到了。或者直接查看Installed Packages目录。Preferences->Browse Packages。


### 3.插件列表：
markdown preview  
html/css/javascript prettify:需要安装nodejs  
BracketHighlighter：高亮显示 “” [] {} 等等  
SideBarEnhancements:侧边栏增强  
Jedi：python自动补全  
Alignment:对齐  
Tomorrow Color Scheme:主题  
Theme-Argonaut:主题  
SublimeCodeIntel:自动补全  

SublimeTmpl 快速生成文件模板
ConvertToUTF8：国人必备啊。解决中文问题  
SublimeLinte：语法检验  
SFtp：强大的ftp工具  
Sublime Prefixr：CSS3 私有前缀自动补全插件，显然也很有用哇  
phpTidy ：是用来格式化php代码的工具.  
Format SQL：格式化sql
Emmet
Trmmer
FileDiffs
DocBlockr


### 4.markdown 

__基本符号__
*,-,+ 3个符号效果都一样，这3个符号被称为 Markdown符号
空白行表示另起一个段落
`是表示inline代码，tab是用来标记 代码段，分别对应html的code，pre标签
__换行__
单一段落( <p>) 用一个空白行
连续两个空格 会变成一个 <br>
连续3个符号，然后是空行，表示 hr横线
__标题__
生成h1--h6,在文字前面加上 1--6个# 来实现
文字加粗是通过 文字左右各两个符号
__引用__
在第一行加上 “>”和一个空格，表示代码引用，还可以嵌套
__列表__
这个是markdown文件的主要表示方式，主题要点化

使用*,+,-加上一个空格来表示
可以支持嵌套
有序列表用 数字+英文点+空格来表示
列表内容很长，不需要手工输入换行符，css控制段落的宽度，会自动的缩放的
__链接__
直接写 [锚文本](url "可选的title")
引用 先定义 [ref_name]:url，然后在需要写入url的地方， 这样使用[锚文本][ref_name]，通常的ref_name一般用数字表示，这样显得专业
简写url：用尖括号包裹url 
这样生成的url锚文本就是url本身
__插入图片__
一行表示: ![alt_text](url "可选的title")
引用表示法: ![alt_text][id],预先定义 [id]:url "可选title"
直接使用<img>标签，这样可以指定图片的大小尺寸
__特殊符号__
用\来转义，表示文本中的markdown符号
可以在文本种直接使用html标签，但是要注意在使用的时候，前后加上空行
文本前后各加一个符号，表示斜体 


__markdown preview__

Markdown Preview较常用的功能是preview in browser和Export HTML in SublimeText。

* 自定义快捷键

如果我们想要直接在浏览器中预览效果的话，可以自定义快捷键：点击 Preferences --> 选择 Key Bindings User，输入：
```
"keys": ["alt+m"], "command": "markdown_preview", "args": { "target": "browser"}
```

* 设置语法高亮和mathjax支持

在Preferences -> Package Settings -> Markdown Preview -> Setting - User中添加如下参数：
```
{
    /*
        Enable or not mathjax support.
    */
    "enable_mathjax": true,

    /*
        Enable or not highlight.js support for syntax highlighting.
    */
    "enable_highlight": true,
}
```
语法高亮跟编辑器的主题有关，可以在Preferences ->Color Scheme找自己喜欢的主题。

* 目录生成
只要文章是按照markdown语法写作的。在需要生成目录的地方写[TOC]即可。

* 在浏览器预览Markdown文档

按Ctrl + Shift + P
输入mp 后回车(Markdown Preview: current file in browser)
此时就可以在浏览器里看到刚才编辑的文档了

* 将markdown文件转换为pdf

还可以使用Pandoc将markdown文件转为pdf
方法如下：安装pandoc、安装MiKTex

1.如果markdown文件中不包含中文字符，可直接使用下面命令转换：  
pandoc infile.md -o outfile.pdf  
2.如果有中文字符，则要先将markdown文档的编码方式改为utf-8，编译pandoc默认的latex引擎是pdflatex，不支持中文，可以手动更改编译用的引擎为xelatex，使用下面命令：  
pandoc infile.md -o outfile.pdf --latex-engine=xelatex

在Sublime Text 2中使用命令：  
Ctrl+Alt+O：在浏览器中预览  
Ctrl+Alt+X：输出为HTML文件  
Ctrl+Alt+C：复制为HTML文件  

### 5.Sublime Text添加插入带当前时间说明

1. 创建插件

Tools → New Plugin:

import datetime  
import sublime_plugin  
class AddCurrentTimeCommand(sublime_plugin.TextCommand):  
    def run(self, edit):  
        self.view.run_command("insert_snippet",   
            {  
                "contents": "%s" % datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")   
            }  
        )  
保存为Sublime Text 2\Packages\User\addCurrentTime.py  

2. 创建快捷键   

Preference → Key Bindings - User:  

[  
    {  
        "command": "add_current_time",  
        "keys": [  
            "ctrl+d"  
        ]  
    }  
]  
