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


### 4.markdown preview

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
