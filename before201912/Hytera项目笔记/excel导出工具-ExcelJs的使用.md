```
    const data = [{
      id: 1,
      name: 'zhou',
    }, {
      id: 2,
      name: 'deng',
    }];
    // 初始化excel
    const workbook = new Excel.Workbook();
    workbook.created = new Date();
    // 生成一个工作表
    const sheet = workbook.addWorksheet('报表');
    sheet.columns = [
      { header: 'Id', key: 'id', width: 10 },
      { header: 'Name', key: 'name', width: 32 },
    ];
    // 添加数据
    sheet.addRows(data);
    // data.forEach(one => {
    //   sheet.addRow(one);
    // });
    // 生成excel文件
    await workbook.xlsx.writeFile('用户报表.xlsx');
```
然后报错： T.createWriteStream is not a function。查资料后发现是因为客户端当前不支持workbook.xlsx.writeFile。

解决办法：
（1）使用writeBuffer， 并使用 file-saver 工具进行保存。
```
import { saveAs } from 'file-saver';
workbook.xlsx.writeBuffer().then(buffer => {
    saveAs(buffer, '用户报表.xlsx');
});
```
这样还是报错，之后查询资料发现应该这样使用：
```
workbook.xlsx.writeBuffer().then(buffer => {
    saveAs(new Blob([buffer], { type: 'application/octet-stream' }), '用户报表.xlsx');
});
```
此处使用到了new Blob()。

Blob相关知识扩展（https://developer.mozilla.org/zh-CN/docs/Web/API/Blob）：
Blob 对象表示一个不可变、原始数据的类文件对象。Blob 表示的不一定是JavaScript原生格式的数据。要从其他非blob对象和数据构造一个 Blob，要使用 Blob() 构造函数。语法：
Blob(blobParts[, options])。返回一个新创建的 Blob 对象，其内容由参数中给定的数组串联组成（也就是说，第一个参数blobParts要是一个数组！）。
用法举例：用字符串构建一个blob
```
var debug = {hello: "world"};
var blob = new Blob([JSON.stringify(debug, null, 2)], {type : 'application/json'});
```
