> Author: JunJianSyu <br />
> Slogan: the myriads of changes

##### 变量类型
```typescript
// number 类型
let num1 : number = 10;
let num2 : number = 20.5;
let a1 : number = Infinity;
let a2 : number = -Infinity
let a3 : number = NaN;
// undefined 类型
let un : undefined = undefined;
// null 类型
let nu : null = null;
// string 类型
let str : string = 'hello'; // 值类型
let str1 : String = new String('hello');
// boolean 类型
let boo : boolean = true;
let boo1 : boolean = false;
// symbol 类型
let sy : symbol = Symbol('bar');
// array 类型
let arr1 : number[] = [1, 2]; // 字面量
let arr2 : Array<string> = ['a', 'b']; // 泛型 相当于数组中每个元素的类型
let arr3 : string[] = new Array('a', 's'); // 构造函数
let arr4 : Array<number | string> = [2, 'a']; // 联合类型
// tuple 元组类型
let tup : [string, number] = ['abc', 123]; // 元组类型长度有限，分别为每一个元素定义类型
// enum 枚举类型
enum REN {
    nan,
    nv,
    yao
}
// 数组下标从0开始
console.log(REN.nan); // 0
console.log(REN[0]) // nan

// 字符串枚举每个成员必须初始化并且无法生成反射
enum SIJI {
    chun = '春',
    xia = '夏',
    qiu = '秋',
    dong = '冬'
}
console.log(SIJI.chun) // 春
// void 类型
function f1() : void {
    console.log('void类型')
}
// any 类型
// 其他类型都是any类型的子类型 ，any类型的值可以被赋值为任何类型的值
// 如果在声明变量时，没有声明其类型，也没有初始化，（因为类型推断会自动判断类型），那么它就会被判断为any类型
let an : any = 'any 类型';
console.log(an); // any 类型
an = 25;
console.log(an); // 25
// 在any类型变量上可以访问任何属性，即使它不存在
an.fn();
// never 类型
// never 表示永远不会存在的值的类型， never 是任何类型的子类型，但是没有任何类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使any也不可以赋值给never。
function f5() : never {
    while (true) {
        // do something
    }
}
// 用于描述从不会有返回值的函数
function x1(str: string) : never {
    throw new Error(str)
}
// 用于描述总抛出错误的函数

// 日期类型
let da : Date = new Date();
console.log(da)

// 正则
let reg1 : RegExp = new RegExp('syu', 'gi');
let reg2 : RegExp = /syu/gi;
```

##### 函数
```typescript
// 函数生命定义
function f(age: number) : String {
    // 参数类型 ： 返回类型
    return `年龄${age}`
}
let age : number = 27;
let res : string = f(age);
console.log(res);

// 函数表达式
let f1 = (age : number) : String => {
    return `年龄${age}`
}
// 表达式定义后，必须调用函数

// 可选参数
function x(x: number, y?: number) : number {
    if (y) {
        return x + y
    } else {
        return x;
    }   
}

// 接口函数
interface int1 {
    asy (age:number) : void;
}

// 函数重载
// 先定义函数格式，最后实现函数方法
function f1(x: number, y: number): number;
function f1(x: string, y: string): string;
function f1(x: any, y: any) : any {
    return x + y;
}
```

##### 类
```typescript
class Syu {
    public name: string;  // 公共修饰符
    private age: number;  // 私有修饰符
    readonly birth: string // 只读修饰符
          
    constructor(name:string, age:number) {
        this.name = name;
        this.age = age;
        this.birth = '2020';
    }
    public say () : void {
        console.log('JunJianSyu')
    }
    private setAge(age: number) : void {
        this.age = age;
    }
}

class newSyu extends Syu{
    constructor(name: string, age: number) {
        super(name, age);
    }
    static name1:string = '静态属性';
    static showName() : void {
        console.log('静态方法')
    }   
}

const newSyuInstance = new newSyu('junjiansyu', 18);
console.log(newSyuInstance.name);

// 多态
class Animal {
    name: string
    constructor(name: string) {
        this.name = name;
    }
    eat() : void {
        // 让子类实现eat
    }
}

class Laohu extends  Animal {
    constructor(name: string) {
        super(name);
    }
    eat() : void {
        console.log(`${this.name}吃肉！`)
    }   
}

class Laoshu extends Animal {
    constructor(name: string) {
        super(name);
    }
    eat() : void {
        console.log(`${this.name}吃肉！`)
    }
}

const laohu : Laohu = new Laohu('老虎');
laohu.eat();
const laoshu : Laoshu = new Laoshu('老鼠');
laoshu.eat();

// 类和接口
interface Play {
    plays (difang:string) : void;
}

class Playy implements Play {
    plays(difang:string) : void {
        console.log(`我们要去${difang}玩！！！`)
    }
}

const  pl : Playy = new Playy();
pl.plays('北京');

// 抽象类和抽象方法
abstract class People {
    name: string
    constructor(name:string) {
        this.name = name;
    }
    abstract eat (food:string) : void; // 抽象方法不包括具体实现，并且必须在派生类中实现
}

class Stud1 extends People {
    constructor(name:string) {
        super(name);
    }
    eat(food:string) : void {
        console.log(`我爱吃${food}`)
    }   
}

const stu11 : Stud1 = new Stud1('syu')
stu11.eat('西红柿');

// 接口
interface Sx {
    name: string
    age: number
};
interface Sta {
    (difang:string, todo: string) : string
};

// 接口属性必须传
function fn(peop:Sx) {console.log(peop)}

const play : Sta = (difang:string, todo:string) : string => { return `${difang}-${todo}`}
```

```typescript
// 接口继承
interface Anmal {
    name: string
    eat (food:string) : void
}

interface LaoHu extends Anmal{
    say(sa: string) : void
}

class XiaoLaoHu implements LaoHu {
    name: string
    constructor(name: string) {
        this.name = name;
    }
    eat(food: string) : void {
        console.log(`${food}`)    
    }
    say(sa: string) : void {
        console.log(`${sa}`)
    }   
}

const xiao: XiaoLaoHu = new XiaoLaoHu('老虎')
xiao.eat('肉')
xiao.say('你好')
```

##### 泛型
```typescript
// 泛型函数
function f1<T>(value:T) : T {
    // 传入参数类型为T，返回值的类型也为T
    console.log(`${value}`)
    return value
}

f1<number>(10)

function f2<T>(value: T) : any {
    console.log(`${value}`);
    return `${value}`
}

console.log(f2<string>('syu'))

// 泛型类
class Ni <T> {
    name: T
    constructor(name: T) {
        this.name = name
    }
    say (value: T) : any {
        return `${this.name}-${value}`
    }
}

const ni1 = new Ni<string>('syu');
ni1.say('你好')

const ni2 = new Ni<number>(20);
ni2.say(23)

// 泛型接口
interface Niface {
    <T> (value: T) : any
}

const fff : Niface = <T>(value: T) : any => {
    return `${value}`
}
fff<number>(18)
fff<string>('syu')

// 泛型约束中使用类型参数
// 在写代码中，可以利用keyof对对象中的属性是否存在做出限定，即只能使用对象中的属性名
function getProperty<T, k extends keyof T>(obj:T, key: k) {
    return obj[key]
}
let obj = { a: 1, b: 2, c: 3}
getProperty(obj, "a"); // true
getProperty(obj, "t"); 
// error: Argument of type '"t"' is not assignable to parameter of type '"a" | "b" | "c"'.

/**
 * 对数组进行排序
 * @param arr Array<object> 对象数组
 * @param key 对象中的键值
 * @param top 取前几
 * @param order 升序或者降序 默认是降序，即从高到低
 * 示例 let arr = [{name: 'kobe', age: 19}, {name: 'james', age: 17}, {name: 'durant', age: 27}]
 * takeTop(arr, 'age', 10)
 * [{name: 'durant', age: 27}, {name: 'kobe', age: 19}, {name: 'james', age: 17}]
 */
function orderBy(arr: any[], key: any[], order: any[]) : any[] {
    return []
}
function take(arr: any[], top: number) : any[] {
    return []
}
function takeTop<T, K extends keyof T>
(arr: T[], key: K, top: number, order: 'desc' | 'asc' = 'desc'): T[] {
    return take(orderBy(arr, [key], [order]), top);
}
```

##### 命名空间
```typescript
namespace FnOne {
    export class Animator {
        
    }
}

namespace FnTwo {
    export class Animator {
    
    }
}

const animator_one : FnOne.Animator = new FnOne.Animator();
const animator_two : FnTwo.Animator = new FnTwo.Animator();
```

##### 类型断言
```typescript
function fn(name: string, age: string | number) : void {
    if ((<string>age).length) { // 断言类型 不然报错
        console.log((<string>age).length)
    } else {
        console.log(age.toString)
    }
}
fn('syu', 18);
// 类型断言，如果联合类型中不存在指定类型是不允许的
```
