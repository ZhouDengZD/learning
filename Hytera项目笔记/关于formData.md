1.创建： var formdata = new FormData();

2.获取/修改 FormData 的三种方式：
（1）创建一个空的FormData对象，然后再用append方法逐个添加键值对
```
var formdata = new FormData();
formdata.append("name", "呵呵");
formdata.append("url", "http://www.baidu.com/");
```
（2）取得form元素对象，将它作为参数传入FormData对象中
```
var formobj =  document.getElementById("form");
var formdata = new FormData(formobj);
```
（3）利用form元素对象的getFormData方法生成它
```
var formdata = formobj.getFormData()
```

FormData.append(name, value, filename)： 向已存在的键**添加**新的值，如该键不存在，新建之。
- name：键 (key)， 对应表单域
- value：表单域的值
- filename (optional)：The filename reported to the server (a USVString), when a Blob or File is passed as the second parameter. The default filename for Blob objects is "blob"
```
formData.append('name', true);
formData.append('name', 74);
formData.getAll('name'); // ["true", "74", ]
```
注: 通过 FormData.append()方法赋给字段的值：(字段的值可以是一个Blob对象,一个File对象,或者一个字符串,剩下其他类型的值都会被自动转换成字符串)。

