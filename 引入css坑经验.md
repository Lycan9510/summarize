
迁移新项目中引入css要使用:
``` 
import '../style.css';
```

错误示例：
``` 
import ('../style.css')
```

原因:
import才是同步引入css,**静态加载资源，编译时放到代码块最顶层**
编译后的css：
``` 
<link href="./style/style.css" rel="stylesheet">
```

import()是异步引入css，会导致css加载延后页面无样式出现闪屏
编译后的css：
``` 
<link rel="stylesheet" type="text/css" href="./style/style.css">
```