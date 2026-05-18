# js内部对象和宿主对象

JS中，可以将对象分为“内部对象”、“宿主对象”和“自定义对象”三种。

### **内部对象**
js中的内部对象包括Array、Boolean、Date、Function、Global、Math、Number、Object、RegExp、String
以及各种错误类对象，包括Error、EvalError、RangeError、ReferenceError、SyntaxError、TypeError。
其中Global和Math这两个对象又被称为“内置对象”，这两个对象在脚本程序初始化时被创建，不必实例化这两个对象。
### **宿主对象**
 - 宿主对象就是执行JS脚本的环境提供的对象。对于嵌入到网页中的JS来说，其宿主对象就是浏览器提供的对象，所以又称为浏览器对象，如IE、Firefox等浏览器提供的对象。不同的浏览器提供的宿主对象可能不同，即使提供的对象相同，其实现方式也大相径庭！这会带来浏览器兼容问题，增加开发难度。
 - 浏览器对象有很多，如Window和Document等等。
 
### **自定义对象**
顾名思义，就是开发人员自己定义的对象。JS允许使用自定义对象，使JS应用及功能得到扩充