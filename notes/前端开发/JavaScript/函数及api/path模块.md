# path模块


js内置包，无需额外下载

#### **引入方法**:

```
import path from 'path'
```
or
```
const path = require('path');
```

- **resolve方法**：

path.resolve方法用于将路径或路径片段序列解析为绝对路径，可以输入多个参数，从最后一个参数开始依次向左执行。

有两种极端情况：如果输入参数没有遍历完但是已经解析出了绝对路径，那么剩下的参数不会执行。如果把所有参数遍历完后还没有解析出绝对路径，那么就会与当前工作目录拼接。

举例：
```
path.resolve('/foo/bar', './baz');
//  ./baz是一个相对路径，首先会与 /foo/bar 进行拼接，最终结果是/foo/bar/baz

path.resolve('/foo/bar', '/tmp/file/');
//  因为/tmp/file/本来就是个绝对路径了，因此不会去执行/foo/bar，直接输出/tmp/file

path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif');
/*  
    如果当前工作目录是 /home/myself/node，
    因为../gif/image.gif是一个相对路径，并且../会对前一个路径参数进行操作，
    因此前一个路径变成static_files，拼接起来得到./static_files/gif/image.gif，
    继续与前一个参数进行拼接，得到./wwwroot/static_files/gif/image.gif，
    依然是一个相对路径，但是已经没有参数了，那么根据规则会拼接当前路径
    得到 /home/myself/node/wwwroot/static_files/gif/image.gif
*/
```
