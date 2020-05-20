#### URL中search参数转json格式
```
    // search参数转json
    searchToJson() {
        let url = location.search; // 获取当前浏览器的URL
        let param = {}; // 存储最终JSON结果对象
        url.replace(/([^?&]+)=([^?&]+)/g, function (s, v, k) {
            v = v.toLowerCase()
            param[v] = decodeURIComponent(k);//解析字符为中文
            return k + '=' + v;
        });
        return param;
    }
```