### dart语法简单汇总
1. 基础知识结合
    1. dart的入口函数为main函数
        ```dart
            // 注意: dart中函数的返回值可省略
            void main(List<String> args) {
                print('hello world');
            }
        ```
    2. print打印内容
    3. dart为强类型语言
    4. sleep(Duration(seconds: 3));
2. 定义变量:
    1. 明确声明变量类型:
        * 获取变量的类型: 变量名.runtimeType
            * 每个声明的变量都会有runtimeType属性
        ```dart
            int num = 100;
            double numD = 10.88;
            String str = 'abc';
        ```
    2. 类型推导声明变量:
        ```dart
            // 自动推导msg变量的理性为String
            var msg = 'hello world';

            // const声明的变量为常量，不能再更改
            const msg = 'hello world'; 
            
            // 声明常量，不可更改
            final msg = 'hello world';

        ```
        * **const跟final的区别:**
            * const 必须直接赋值常量
            * final 可以在运行时进行赋值
                ```dart
                    int getNum() => 10;
                    final num = getNum(); // 正常运行，不会报错

                    const num = getNum(); // 报错，const 只能直接赋值为10
                    const num = 10;
                ```
    3. dynamic声明: 声明的变量可赋值不同类型的值
        ```dart
            dynamic msg = 'hello world';
            msg = 100;
        ```
















3. 变量类型:
    1. int/double
        * 数值跟字符串之间的相互转化
            ```dart
                // 字符串转数值
                int num = int.parse('1'); // 1 int
                double numd = double.parse('1.1'); // 1.1 double

                // 数值转字符串
                String s = num.toString(); // 1 String

                // 保留两位小数，四舍五入
                double d = 1.2356;
                String ss = d.toSringAsFixed(2) // 1.24 String 
            ```
    2. bool
        * dart中没有非空即真
            ```dart
                String msg = 'abc';
                if(msg) { // 错误写法

                }
            ```
    3. String
        * 双引号或单引号表示字符串
        * `'''...'''`表多行字符串
        * `${}:`字符串与变量拼接
            * 如果变量是一个标识符，则`{}`可省略
            * 如果是一个表达式调用，则`{}`不可省略
            ```dart
                int getNum() => 10;
                String msg = 'msg';

                String res = 'msg: $msg type: ${msg.runtimetype} num: ${getNum()}';
            ```
    4. 集合: List/Set/Map
        ```dart
            // list
            List<int> list = [1, 2, 3];

            // Set
            Set<String> set = {'a', 'b', 'c'};
            // 利用Set对List去重
            List.from(Set.from(list))

            // Map
            Map<String, dynamic> map = {
                'name': '王一扬',
                'age': 20,
                'isMarried': true
            } 
        ```
        * 集合常用操作
            * length: 三种集合都支持length属性
            * List
                * add
                * remove   根据value移除
                * removeAt 根据索引index移除
                * contains
                * removeRange: 根据开始和结尾索引移除元素，会改变原数组
            * Map
                * map[key] 根据key读取值
                * map[key] = value 赋值操作
                * map.entries
                * map.keys
                * map.values
                * map.containsKey
                * map.containsValue
                * map.remove 根据key移除属性

4. 函数Function: 面向对象，函数也是对象
    * 定义: 
        ```dart
            返回值 函数名(参数...) {
                函数体
                return 返回值
            }
            // 例:
            int getSum(int a, int b) {
                int sum = a + b;
                return sum;
            }
        ```
        * dart中函数参数类型可省略
            ```dart
                int getSum(a, b) {
                    return a + b;
                }
            ```
    * 箭头语法: 函数体只有一个表达式
        ```dart
            int getSum(a, b) => a + b; 
        ```
    * 函数参数:
        * 必传参数:
        * 可选参数: [] / {}
            * 位置可选参数: []
                ```dart
                    // []中括号里面的参数b和参数c为可选参数
                    void showStr(a, [b, c]) {
                        print('$a $b $c');
                    }

                    showStr('a');
                    showStr('a', 'b');
                    showStr('a', 'b', 'c');
                ```
            * 命名可选参数: {}
                ```dart
                    void showStr(a, {name, age}) {
                        print('$a $name $age');
                    }

                    showStr('a');
                    showStr('a', name: 'b');
                    showStr('a', name: 'b', age: 'c');
                ``` 
                * @required 修饰命名参数为必传
                    * @required为flutter中的装饰器
                    ```dart
                        // name即为必传的命名参数
                        void showStr(a, {@required name}) {
                            print('$a $name');
                        }
                    ```
        * 默认参数
            * 可选参数才能设置默认值，必传参数不能设置默认值
                ```dart
                    void showStr(name, [age = 27]) {}
                    void showStr(name, {age = 27}) {}

                    // 不能给必传参数name设置默认值 会报错
                    void showStr(name = 'wyy', [age]) {}
                ```
    * 函数是一等公民:
        * 可以将函数赋值给变量，也可将函数作为参数或者函数返回值来使用
    * 匿名函数lambda
        * 常跟函数式编程一起使用
            ```dart
                List<int> list = [1,2,3].map((item) => item + 10).toList();
            ```
    * 作用域:
        * dart中作用域跟js类似，`{}`优先使用自己的作用域中的变量，没有的话再向外面一层一层的查找
            ```dart
                var name = 'global';
                main(List<String> args) {
                    // var name = 'main';
                    void foo() {
                        // var name = 'foo';
                        print(name);
                    }

                    foo();
                }

            ```
        * 闭包
            ```dart
                main(List<String> args) {
                    makeAdder(num addBy) {
                        return (num i) {
                        return i + addBy;
                        };
                    }

                    var adder2 = makeAdder(2);
                    print(adder2(10)); // 12
                    print(adder2(6)); // 8

                    var adder5 = makeAdder(5);
                    print(adder5(10)); // 15
                    print(adder5(6)); // 11
                }
            ```
        * 函数如果没有指定返回值，则默认返回null;


5. 运算符
    * `~/` 整除 (3 ~/ 2 结果为1)
    * `??=` 赋值操作: 但变量为null时，才进行赋值操作
        ```dart
            var a = null;
            a ??= 'a'; // a

            var b = 'b';
            b ??= 'bb' // b
        ```
    * `??`条件运算符，类比js中的`||`
        ```dart
            var a = null ?? 'a'; // a
            var b = 'b' ?? 'bb'; //b
        ```
    * `..`级联操作，链式操作
        ```dart
            List()
                ..add(1)
                ..add(2)
                ..add(3);
        ```
6. 流程控制:
    * if...else(dart中不支持非空即真和非0即真)
    * switch...case
        ```dart
            var temp = 'a'
            switch(temp) {
                case 'a':
                    print('a');
                    break;
                case 'b':
                    print('b');
                    break;
                default:
                    print('...');
                    break;
            }
        ```
    * 循环操作:
        * for
            ```dart
                for(int i = 0; i < 10; i++) {}
            ```
        * for in
            ```dart
                for(String item in ['a', 'b', 'c']) {}
            ```
        * while/do while
        * continue/break与js一致
7. 类和对象
    * 基础知识:
        * class关键字来定义对象
        * 注意: dart开发风格中，方法中需要使用属性时，一般会**省略this**
        * 在创建实例时，new关键字可省略
            ```dart
                class Person {
                    String name;
                    void showName() {
                        // 注意: 此处省略了this
                        print(name);
                    }
                }
                Person p = Person();
                p.name = 'wyy';
                p.showName();
            ```
        * dart中没有函数重载: 不能有两个相同名称的函数同时存在
    * 构造对象: 
        1. 普通构造方法:
            * 当根据类创建实例时，会调用构造方法；
            * 如果没有声明构造方法，该类会默认有一个无参的构造方法
                ```dart
                    class Person {
                        String name;
                        int age;
                        Person(String name, int age) {
                            this.name = name;
                            this.age = age;
                        }

                        // 重写了toString方法
                        @override
                        String toString() => 'name=$name age=$age';
                    }

                    // 构造函数语法糖(常用)
                    Person(this.name, this.age)
                    // 等价于
                    Person(String name, int age) {
                        this.name = name;
                        thia.age = age;
                    }
                ```
        2. 命名构造函数: 同一个类创建多个构造函数
            * Person.fromMap(map)
                ```dart
                    class Person {
                        String name;
                        int age;
                        Person(this.name, this.age);
                        // 命名构造函数(很常用)
                        Person.fromMap(Map<String, dynamic> map) {
                            this.name = map['name'];
                            this.age = map['age'];
                        }
                    }
                ```
        3. 初始化列表:
            * 根据已知类属性，计算得出其它的类属性
            * Peron(this.name, this.age): isChild=age<18, isChina=name.length<=3
                * 根据属性name和age得出其它属性isChina和isChild
                ```dart
                    class Person {
                        String name;
                        int age;
                        bool isChild;
                        bool isChina;
                        Person(this.name, this.age): isChild=age<18, isChina=name.length<=3;
                    }
                ```
        4. 重定向构造方法:
            * 一个构造方法去调用另外一个构造方法
                ```dart
                    class Person {
                        String name;
                        int age;
                        Person(this.name, this.age);
                        // 利用this去调用另外一个构造方法
                        Person.fromName(String name): this(name, 27)
                    }
                ```
        5. 常量构造方法:
            * 属性必须用final进行修饰
            * const来修饰构造方法
                ```dart
                    class Person {
                        final String name;
                        const Person(this.name);
                    }

                    Person p1 = const Person('wyy');
                    Person p2 = const Person('wyy');
                    // 判断两对象是否为同一对象;
                    identical(p1, p2) // true
                ```
        6. 工厂构造函数
            * factory关键字来修饰构造函数
            ```dart
                main(List<String> args) {
                    var p1 = Person('why');
                    var p2 = Person('why');
                    print(identical(p1, p2)); // true
                }

                class Person {
                    String name;

                    static final Map<String, Person> _cache = <String, Person>{};

                    factory Person(String name) {
                        if (_cache.containsKey(name)) {
                            return _cache[name];
                        } else {
                            final p = Person._internal(name);
                            _cache[name] = p;
                            return p;
                        }
                    }

                    Person._internal(this.name);
                }
            ```
    * setter和getter
        * dart中类定义的属性是可以直接被外界访问的
        * 如果需要监控类属性被访问或修改，可使用getter/setter
        * 注意getter和setter不是Function，而是普通的类属性，直接通过点语法进行访问
            ```dart
                class Person {
                    String name;
                    Person(this.name);
                    String get getName => name;
                    set setName(String name) => this.name = name;
                }

                Person p = Person('wyy');
                p.getName; // 'wyy'

                p.setName = 'muzi';
                p.getName; // 'muzi'
            ```
    * 类的继承:
        * extends 关键字实现继承
        * 继承是多态的使用前提
        * super 关键字，子类中访问父类
        * @override 子类可以对父类的方法进行重写
        * 子类的构造方法执行之前，隐式调用父类的默认无参构造方法
        * 通过super关键字，显式调用父类声明的构造函数
            ```dart
                class Animal {
                    int type;
                    Animal(this.type);
                    void run() => print('run...');
                }

                class Person extends Animal {
                    String name;
                    int age;
                    // 初始化列表的运用及构造语法糖合用
                    // super关键字显式调用父类构造方法
                    Person(this.name, this.age, int type): super(type);
                    
                    // 重写父类run方法
                    @override
                    void run() => print('Person run...');
                }
            ```
    * 抽象类/抽象方法: abstract
        * 抽象方法只能存在于抽象类中
        * 抽象类是使用abstract声明的类
        * 抽象类不能实例化
        * 抽象类中的抽象方法必须被子类全部实现，抽象类中已经实现的方法，可以不被子类实现
        * 在开发中，通常将用于给别人实现的类声明为抽象类，implements
            ```dart
                abstract class Shape {
                    getArea();
                }

                class Circle extends Shape {
                    double r;

                    Circle(this.r);

                    @override
                    getArea() {
                        return r * r * 3.14;
                    }
                }

                class Reactangle extends Shape {
                    double w;
                    double h;

                    Reactangle(this.w, this.h);

                    @override
                    getArea() {
                        return w * h;
                    }
                }

                Shape c = Circle(10);
                c.getArea(); // 314
                Shape r = Reactangle(10, 10)
                r.getArea(); // 100
            ```
            * 注意: 在通过implements实现某个类时，类中所有的方法都必须被重新实现(无论这个类原来是否已经实现过该方法)
            ```dart
                abstract class Runner {
                    run();
                }

                abstract class Flyer {
                    fly();
                }
                // 实现关键字: implements
                class SuperMan implements Runner, Flyer {
                    @override
                    run() {
                        print('超人在奔跑');
                    }

                    @override
                    fly() {
                        print('超人在飞');
                    }
                }
            ```
        * implements 跟 extends: 前者可多实现，而后者只能单继承
    * Mixin混入:
        * 子类可使用父类中的方法，而无需继承，解决dart中单继承的问题
        * mixin关键字用于定义可混入的类
        * 用with关键字进行混入
            ```dart
                main(List<String> args) {
                    var superMan = SuperMain();
                    superMan.run();
                    superMan.fly();
                }

                mixin Runner {
                    run() {
                        print('在奔跑');
                    }
                }

                mixin Flyer {
                    fly() {
                        print('在飞翔');
                    }
                }

                // implements的方式要求必须对其中的方法进行重新实现
                // class SuperMan implements Runner, Flyer {}

                class SuperMain with Runner, Flyer {

                }
            ```
    * 类成员/类方法: static关键字
        * 可直接通过类名访问的属性或方法
        * 静态方法只能使用静态属性
            ```dart
                class Person {
                    static String name;
                    // name为类属性，而不是实例属性
                    static getName() => name;
                }

                Person.name = 'wyy';
                Person.getName(); // wyy
            ```
8. 枚举类型: enum关键字定义枚举类型
    * index: 表示每个枚举常量的索引，从0开始
    * values: 包含所有枚举值的List
        ```dart
            enum Colors {
                red,
                yellow,
                blue
            }
            
            Colors.red.index // 0
            Colors.yellow.index // 1

            // [ColorsMy.red, ColorsMy.yellow, ColorsMy.blue]
            Colors.values 
        ```
9. 泛型:
    * List 的泛型定义:
        ```dart
            const list = [1, 'a', true];
            List<dynamic> list = [1, '2', true];
        ```
    * Map 的泛型定义:
        ```dart
            const map = {'one': 'a', 'two': 1, 'three': true};
            Map<String, dynamic> map = {'one': 'a', 'two': 1, 'three': true};
        ```
    * 类的泛型 T:
        ```dart
            class Location<T> {
                T lat;
                T lon;
                Location(this.lat, this.lon);
            }
            Location l = Location<int>(10, 10);
            Location l1 = Location<String>('1', '10');

            // 限制泛型类型
            class Location<T extends num> {
                // T extends num限制了泛型只能为num类型
                T lat;
                T lon;
                Location(this.lat, this.lon);
            }

            Location l = Location<String>('1', '10'); // 会报错
        ```
    * 方法泛型 T:
        ```dart
            T getName<T>(T name) => name;

            // 调用
            getName('wyy') // 自动推导泛型
            getName<String>('wyy') // 显式声明泛型的类型
        ```
10. dart中的库
    * Dart中任何一个dart文件都是一个库，即使你没有用关键字library声明
    1. 库的导入 import
        ```dart
            import '库所在的uri';
        ```
        * 导入dart标准库:
            * dart:为开头
            * dart:io
            * dart:html
            * dart:math
            * dart:core(dart核心库，默认导入，可省略)
            ```dart
                import 'dart:io'
            ```
        * 导入相对路径的库
            * 通常是自定义的其它dart文件
                ```dart
                    import 'lib/demo.dart';
                ```
        * Pub包管理库，包含一些第三方库
            * package: 为开头
                ```dart
                    //Pub包管理系统中有很多功能强大、实用的库，可以使用前缀 package:
                    import 'package:flutter/material.dart';
                ```
    2. show/hide 显示或隐藏导入库中某些文件
        * show: 显示某个成员，屏蔽其它
        * hide: 隐藏某个成员，显示其它
        ```dart
           import 'lib/student/student.dart' show Student, Person;

           import 'lib/student/student.dart' hide Person;     
        ```
    3. as 关键字（添加命名空间）
        ```dart
            import 'demo.dart' as Demo;
            // 添加命名空间对库中变量进行访问，防止变量名冲突
            Demo.name
            Demo.age
        ```
    4. export 对库进行拆分
        ```dart
            library utils;

            export "mathUtils.dart";
            export "dateUtils.dart";

            // 使用时只需引入utils库即可使用mathUtils和dateUtils提供的所有方法
        ```

11. dynamic跟Object的区别
    * dart中所有的类都默认继承于Object
    * Object已经明确了变量的类型，而dynamic是在运行时确定变量的类型
    * dynamic动态类型: 
        * 会在程序运行时确定变量的类型
        * 通常情况下很少使用dynamic，因为类型的不确定性，会存在潜在的危险
        * dynamic常用语Map<String, dynamic>
        ```dart
            Object name1 = 'wyy';
            dynamic name2 = 'wyy';

            name1.length; // 会报错，因为Object类型不存在length属性

            name2.length; // 3， dynamic在运行时会确定name2为String类型，拥有length属性
        ```
12. dart异步:
    * 跟js类似: **单线程+事件循环**， Event Loop
    * 非阻塞式调用: 调用执行后，程序不会停止，会等待结果返回值
    * 事件循环: 
        * 将需要处理的一系列事件(点击、IO、网络请求...)放到事件队列中
        * 主线程结束后，不断的从事件队列中取出事件，进行执行，执行完毕，从事件队列中清空执行事件
        * 先进先出
    * dart中的异步操作: Future以及async/await
        * Future类比于promise
            ```dart
                Future<String> getNetworkData() {
                    return Future<String>(() {
                        sleep(Duration(seconds: 3));
                        return "network data";
                    });
                }

                print('1');
                var future = getNetworkData();
                future.then((str) => print(str));
                print('2');
                // 打印结果: 1, 2, network data
            ```
            * 抛异常: .catchError
                ```dart
                    Future<String> getNetworkData() {
                        return Future<String>(() {
                            sleep(Duration(seconds: 3));
                            throw Exception('网络出现异常');
                        });
                    }

                    var f = getNetworkData();
                    f.then((res) => print(res))
                        .catchError((res) => print(res));
                ```
            * Future有两种状态: 
                * uncompleted: 未完成状态，Future执行内部操作，如网络请求
                * completed: 已完成状态， 操作完成，返回一个值，或抛出异常
            * 同时Future也支持链式调用
                ```dart
                    getNetworkData().then((value1) {
                        print(value1);
                        return "content data2";
                    }).then((value2) {
                        print(value2);
                        return "message data3";
                    }).then((value3) {
                        print(value3);
                    });
                ```
            * Future.value(value): 获取一个完成状态的Future，并将value值传递给回调函数
                ```dart
                     Future.value("哈哈哈").then((value) {
                        print(value);
                    });
                ```
            * Future.error(object): catchError异常状态的Future
                ```dart
                    Future.error(Exception("错误信息")).catchError((error) {
                        print(error);
                    });
                ```
            * Future.delayed(时间, 回调函数): 延迟多长时间再执行回调函数
                ```dart
                     Future.delayed(Duration(seconds: 3), () {
                        return "3秒后的信息";
                    }).then((value) {
                        print(value);
                    });
                ```
        * async/await几乎与es7中一致
            * async函数的必须是**Future**
            * async函数会返回一个Future
                ```dart
                    Future<String> getNetworkData() {
                        return Future<String>(() {
                            sleep(Duration(milliseconds: 3000));
                            return 'muzi';
                        });
                    }

                    void _showDartBasic() async {
                        print('start');
                        var f = await getNetworkData();
                        print(f);
                        print('end');
                    }

                    _showDartBasic();
                    // start, muzi, end
                ```
            * try...catch捕获异常:
                ```dart
                    Future<String> getNetworkData() {
                        return Future<String>(() {
                            sleep(Duration(milliseconds: 3000));
                            // return 'muzi';
                            throw Exception('网络发生异常');
                        });
                    }

                    void _showDartBasic() async {
                        print('start');
                        try {
                            await getNetworkData();
                        } catch (e) {
                            print('error: $e');
                        }
                        print('end');
                    }
                ```

    * 加载本地json文件:
        ```dart
            // 注意: assets/my.json资源路径需要在pubspec.yaml中配置
            Future<List> getListByJson() async{
                String s = await rootBundle.loadString('assets/my.json');
                return json.decode(s); // 类比json.parse()
            }
        ```
    * 面试题: 代码执行顺序问题:(跟js中的Promise还是有区别的)
        ```dart
            import "dart:async";

            main(List<String> args) {
                print("main start");

                Future(() => print("task1"));
                    
                final future = Future(() => null);

                Future(() => print("task2")).then((_) {
                    print("task3");
                    scheduleMicrotask(() => print('task4'));
                }).then((_) => print("task5"));

                future.then((_) => print("task6"));
                scheduleMicrotask(() => print('task7'));

                Future(() => print('task8'))
                    .then((_) => Future(() => print('task9')))
                    .then((_) => print('task10'));

                print("main end");
            }

            main start
            main end
            task7
            task1
            task6
            task2
            task3
            task5
            task4
            task8
            task9
            task10
        ```
    