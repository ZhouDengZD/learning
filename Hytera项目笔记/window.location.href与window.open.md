#### location.href常见的几种形式
```
window.location.href; // 当前页面打开URL页面
this.location.href; // 当前页面打开URL页面
location.href; // 当前页面打开URL页面
self.location.href; // 当前页面打开URL页面
parent.location.href; // 在父页面打开新页面
top.location.href; // 在顶层页面打开新页面 
```
注：
（1）如果页面中自定义了frame，那么可将parent、self、top换为自定义frame的名称,效果是在frame窗口打开url地址
（2）window.location.href=window.location.href; 和window.location.Reload(); 都是刷新当前页面。区别在于是否有提交数据。当有提交数据时，window.location.Reload()会提示是否提交，window.location.href=window.location.href;则是向指定的url提交数据。
（3）location是window对象的属性，而所有的网页下的对象都是属于window作用域链中（这是顶级作用域），所以使用时是可以省略window。而top是指向顶级窗口对象，parent是指向父级窗口对象

#### window.location与window.open的区别

（1）window.location是window对象的属性，而window.open是window对象的方法。window.location是对当前浏览器窗口的URL地址对象的参考！window.open是用来打开一个新窗口的函数！ 也就是说：window.open()会打开新页面，但是用window.location.href＝"" 却是在原窗口打开的。有时浏览器会一些安全设置window.open肯定被屏蔽。例如避免弹出广告窗口。

（2）window.open不一定是打开一个新窗口。只要有窗口的名称和window.open中第二个参数中的一样就会将这个窗口替换，用这个特性的话可以在iframe和frame中来代替location.href。 如：
<iframe name="aa"></iframe>   

<input type=button   οnclick="window.open('1.htm','aa','')">
和   
<input type=button   οnclick="self.frames['aa'].location.href='1.htm'">的效果一样。

（3）window.open()可以在一个网站上打开另外的一个网站的地址，而window.location()只能在一个网站中打开本网站的网页。