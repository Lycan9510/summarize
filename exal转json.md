### node.js exal文件转json文件
```
var xlsx = require("node-xlsx");
var fs = require('fs');
var old_file_name = "xxx.xlsx";
var new_file_name = "xxx.json";
var list = xlsx.parse(old_file_name); // 需要 转换的excel文件

// 数据处理 方便粘贴复制
var data = list[0].data;  // 1.读取json数据到变量暂存
var len = data.length;
var test_list = [];
var key_list = data[0];
for(var i = 1; i < len; i ++){  // 2.数据处理
    var item = data[i];
    if (!item.length) break; // 空行 跳出循环
	let test_msg = {};
    key_list.forEach((value, index) => {
        test_msg[value] = item[index];
    })
    test_list.push(test_msg);
}

writeFile(new_file_name,JSON.stringify(test_list)); // 输出的json文件  3.数据写入本地json文件

function writeFile(fileName,data)
{  
  fs.writeFile(fileName,data,'utf-8',complete);  // 文件编码格式  utf-8
  function complete(err)
  {
      if(!err)
      {
          console.log("文件生成成功");  // 终端打印这个 表示输出完成
      }   
  } 
}
```