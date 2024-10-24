## hugo 网站目录

### archetypes
使用hugo new xx.md来创造一个新页面时，会根据该目录下的default.md模板生成新文件。

### assets 
资源目录，只有那些 .Permalink 和 .RelPermalink 引用的文件会发布到 public 目录中

#### content
存放md文件，该目录下可以自行创建若干个子目录来便于对文章进行分类，这些子目录被称为section
* _index.md 生成目录页，使用对应的section.html模板
* index.md 生成内容页，使用对应的single.html模板
* md文件顶部用 ---- 包围的内容为前置参数，在生成html页时用
* <!--more--> 前面的为内容摘要，在文章列表中显示
* md文件顶部的date lastmod不要超过当前机器时间，否则整个目录不输出！！！教训

### layouts
* 存放用于渲染html页面的模板文件
* layouts/_default/baseof.html 是生成网站所有页面的模板，通过 partial 和 block 进行区域扩展
* 站点首页模板 layouts/index.html
* 列表页模板：多级目录时首先查找对应目录的section.html，缺省使用layouts/_default/section.html
* 详细页模板：多级目录时首先查找对应目录的single.html，缺省使用layouts/_default/single.html
* block "blockname"：将当前页模板中定义的(define "blockname")内容替换到当前位置
* partial "xx.html"：运行partials目录下的"xx.html"文件脚本，生成的字符串替换到当前位置

### static
* 存放静态内容：图片、css、js等，发布时目录下的所有文件及子目录复制到public目录下
* favicon.ico 浏览器收藏或标签的头像，可通过favicon-32x32.png在线生成
* safari-pinned-tab.svg 通过android-chrome-512x512.png在线生成
* site.webmanifest PWA用到

### public
生成静态站点的文件输出目录

### resources
hugo 编译时用于缓存某些文件来提高生成效率。


## html模板语法
模板语法都包含在 {{ }} 中间，{{.}} 中的点表示当前对象
管道符： | ，|前面的命令会将运算结果(或返回值)传递给后一个命令
变量赋值：{{ $src := .Src }}
去除前后的空格：{{- .Name -}}，- 要紧挨{{和}}
条件判断：{{if pipeline}} T1 {{else if pipeline}} T0 {{end}}
循环：{{range pipeline}} T1 {{end}} ，如果pipeline的值其长度为0，不会有任何输出
节省对象前缀：{{with pipeline}} {{ .属性 }} {{end}}，如果pipeline为empty不产生输出

### 全局函数：
default 未设置值时返回的默认值，通常放在管道的最后提供缺省值
dict 创建一个字典，dict 键 值 [键二] [值二]
delimit 以一个字符串分割数组元素并输出，delimit 数组 分隔符
eq 比较两变量，返回布尔值，eq 变量1 变量2
first/last 节选（排序后的）前/后X个元素，first 数量
