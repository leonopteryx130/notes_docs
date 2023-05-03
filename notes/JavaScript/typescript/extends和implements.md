# extends和implements

在ts中，extends和es6 class继承的extends是一样的，ts里的接口interface也可以用extends关键字实现继承，继承者和被继承者可以是interface也可以是class，举几个例子：
```
interface IPeoPle extends IPerson { // 接口继承接口
    sex: string;
}

class User { 
    age: number;
    name: string;
}
interface IRoles extends User{ // 接口继承类
}

class Point6 {
    x: number;
    y: number;
}
interface Point3d extends Point6 { // 接口继承类
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
class Roles extends User{ // 类继承类

}
```
implements和extends很相似，A implements B，A 必须是class，不能是interface，并且A可以重写B的属性，这点在extends里是不允许的（extends最多只允许重写方法）