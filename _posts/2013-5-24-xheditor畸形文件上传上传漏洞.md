---
layout: post
title: xheditor畸形文件上传上传漏洞
---

#xheditor畸形文件上传上传漏洞

这几天学校有个我以前弄的网站突然不显示内容了,右键一下发现被人改源码了,放进一些链接进去.
回去服务器看了下,发现这个子站的根目录下几乎所有的asp文件都被改了,然后发现了几个奇怪的文件，后缀是.asp;.jpg，卧槽，这是能越过正则的啊。然后系统识别的是asp文件。。同文件名的还有几个txt文件。
![](http://fakefish.me/img/2013-5-24.png)

然后我google了一下，发现很多都是php的解决方法，而且不通用，都是一个内容抄来抄去的，我看到有个黑名单选项

>NoAllowExt=""

然后把这个后缀加上，编辑器就会拒绝上传了。

>NoAllowExt=".asp;jpg"

但是这造成的后果就是jpg也不能上传了,纠结了一中午,发现一处

<pre>
'取得文件的后缀名
Public Function GetFileExt(FullPath)
  If FullPath &lt;&gt; "" Then
    GetFileExt = LCase(Mid(FullPath,InStrRev(FullPath, ".")+1))
    Else
    GetFileExt = ""
  End If
End function
</pre>

把这里的InStrRev替换为InStr就好了，后果就是以后上传的文件中不能含两个以上的'.'了,想想还是算了,改回去,然后在if里加了条 

<pre>
	if FullPath &lt;&gt; "" and instr(FullPath,";")&lt;0 then
</pre>

他娘的没效果..默默地改回去