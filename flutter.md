### 重识flutetr
1. flutter最基本代码语法讲解
    ```dart
        import 'package:flutter/material.dart';
        //主函数（入口函数）
        void main() => runApp(MyApp());
        // 声明MyApp类
        class MyApp extends StatelessWidget{
            //重写build方法
            @override
            Widget build(BuildContext context){
                //返回一个Material风格的组件
                return MaterialApp(
                    title: 'Welcome to Flutteraa',
                    home: Scaffold(
                        //创建一个Bar，并添加文本
                        appBar: AppBar(
                            title:Text('Welcome to Flutter'),
                        ),
                        //在主体的中间区域，添加一个hello world 的文本
                        body: Center(
                            child:Text('Hello World'),
                        ),
                    ),
                );
            }
        }
    ```
    * dart是面向对象的语言；即使是函数也是对象，并且属于function类型的对象
    * void main() => runApp(MyApp()) 相当于
    * void main() { return runApp(MyApp()) } ( 用箭头=>省略了 { } )
    * StatelessWidget: 不可变状态的窗口组件
    * StatefulWidget: 可变状态的窗口组件
        * 一个SattefulWidget会对应一个State类，State表示对应的StatefulWidget要维护的状态
        * 继承自State的状态类通过**widget**关键字访问传递的变量
        * State类的生命周期:
            * initState(): 组件初始化时被调用
            ```dart
                class Home extends StatefulWidget {
                    final String title = 'wyy';
                    @override
                    _HomeWidgetState createState() => _HomeWidgetState();
                }

                class _HomeWidgetState extends State<Home> {
                    List<int> list = [];

                    // 数据初始化
                    @override
                    void initState() {
                        super.initState();
                        list
                            ..add(1)
                            ..add(2)
                            ..add(3);
                    }

                    @override
                    Widget build(BuildContext context) {
                        return Scaffold(
                            child: Center(
                                // 使用widget.title去访问Home类中的变量
                                child: Text(widget.title)
                            )
                        )
                    }
                }
            ```
2. 常用组件Widget
    1. Text:
        * TextAlign属性: 文本对齐方式
            ```dart
                child: Text(
                    'Hello World',
                    textAlign: TextAlign.center
                )
            ```
        * MaxLines属性: 最多显示的行数
            ```dart
                child: Text(
                    'Hello World',
                    textAlign: TextAlign.center
                    maxLines: 1
                )
            ```
        * overflow属性: 文本溢出的，展示状态
            * clip: 直接截取
            * ellipsis: 超出显示省略号
            * fade: 上下的渐变消失效果
            ```dart
                child: Text(
                    'Hello World',
                    textAlign: TextAlign.center,
                    maxLines: 1,
                    overflow: TextOverflow.ellipsis
                )
            ```
        * style属性: 文字字体大小、颜色、粗细...
            ```dart
                child: Text(
                    'Hello World',
                    textAlign: TextAlign.center,
                    maxLines: 1,
                    overflow: TextOverflow.ellipsis,
                    style: TextStyle(
                        fontSize: 25,
                        fontWeight: FontWeight.bold,
                        color: Color.fromARGB(255, 255, 150, 150),
                        decoration: TextDecoration.underline,
                        decorationStyle: TextDecorationStyle.solid,
                    )
                )
            ```
            * [官方文档](https://docs.flutter.io/flutter/painting/TextStyle-class.html)
    2. Container: Container用的最多的组件，相当于div标签
        * Alignment属性: child中的元素对齐方式
            ```dart
                body: Container(
                    alignment: Alignment.centerLeft,
                    child: Text('Hello World')
                )
            ```
            * bottomCenter
            * bottomLeft
            * bottomRight
            * center
            * centerLeft
            * centerRight
            * topLeft
            * topCenter
            * topRight
        * width、height、color(背景颜色)
            ```dart
                body: Container(
                    alignment: Alignment.center,
                    height: 400,
                    width: 400,
                    color: Colors.lightBlue
                )
            ```
        * padding
            ```dart
                // EdgeInsets.all(10)
                // EdgeInsets.fromLTRB(10, 10, 10, 10)
                body: Container(
                    alignment: Alignment.topRight,
                    height: 400,
                    width: 400,
                    color: Colors.lightBlue,
                    padding: EdgeInsets.fromLTRB(10, 20, 30, 40)
                )
            ```
        * margin
            ```dart
                // EdgeInsets.all(10)
                // EdgeInsets.fromLTRB(10, 10, 10, 10)
                body: Container(
                    alignment: Alignment.topRight,
                    height: 400,
                    width: 400,
                    color: Colors.lightBlue,
                    padding: EdgeInsets.fromLTRB(10, 20, 30, 40)
                    margin: EdgeInsets.fromLTRB(10, 20, 30, 40)
                )
            ```
        * deroration: 装饰器，主要设置背景及边框
            * **注意: 设置了decoration这个属性，就不要再设置color，会冲突**
            * **BoxDecoration()**
            ****
            ```dart
                child:Container(
                    child: Text('Hello JSPang',style: TextStyle(fontSize: 40.0),),
                    alignment: Alignment.topLeft,
                    width:500.0,
                    height:400.0,
                    padding:EdgeInsets.fromLTRB(10.0,30.0,0.0,0.0),
                    margin: EdgeInsets.all(10.0),
                    decoration: BoxDecoration(
                        // 设置渐变背景色
                        gradient:const LinearGradient(
                            colors:[
                                Colors.lightBlue,
                                Colors.greenAccent,    
                                Colors.purple
                            ]
                        ),
                        // 设置border边框
                        border: Border.all(
                            width:2.0, 
                            color:Colors.red
                        )
                    ),
                ),
            ```
        * Container容器内嵌Center容易会默认居中:
            ```dart
                child: Container(
                    height: 100,
                    width: 100,
                    child: Center(
                        // 此处文本wyy会在Container容器中居中显示
                        child: Text('wyy')
                    )
                )
            ```
    3. Image: 
        * 加载图片的几种方式:
            * Image.asset: 加载资源图片，会增大打包体积，相对路径
            * Image.network: 网络资源图片
            * Image.file: 加载本地图片，绝对路径，不会增大打包体积
            ```dart
                child: Container(
                    child: Image.network(
                        'http://jspang.com/static/myimg/blogtouxiang.jpg',
                        scale: 1
                    ),
                    height: 100,
                    width: 100
                )
            ```
        * fit属性:
            * BoxFit.fill: 全图展示，图片拉伸，会充满父盒子
            * BoxFit.contain: 全图展示，图片显示原比例，可能会存在空隙
            * BoxFit.cover: 显示可能拉伸，可能裁切，充满（图片要充满整个容器，还不变形）。
            * BoxFit.fitWidth：宽度充满（横向充满），显示可能拉伸，可能裁切。
            * BoxFit.fitHeight ：高度充满（竖向充满）,显示可能拉伸，可能裁切
            * BoxFit.scaleDown：效果和contain差不多，但是此属性不允许显示超过源图片大小，可小不可大。
            ```dart
                // 注意: 如果设置cover后，图片没有充满，则需要设置
                // width: double.infinity
                // height: double.infinity
                child: Image.network(
                    'https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1576644564&di=3a9ffc83ced918f9dc050cf7e3e655b8&src=http://pic1.win4000.com/mobile/2019-10-31/5dbaa3f8b8be2.jpg',
                    width: double.infinity,
                    height: double.infinity,
                    fit: BoxFit.cover,
                )
            ```
        * repeat属性:
            * ImageRepeat.repeat: 图片横向和纵向都重复，直到铺满整个画布
            * ImageRepeat.repeatX: 横向重复
            * ImageRepeat.repeatY: 纵向重复
            ```dart
                child: Image.asset(
                    'https://...',
                    repeat: ImageRepeat.repeat
                )
            ```
    4. ListView: 列表组件
        * ListView组件的简单声明使用
            ```dart
                child: ListView(
                    // children属性为一个Widget组件数组
                    children: <Widget>[
                        // ListTile组件(列表瓦片)
                        ListTile(
                            leading: Icon(Icons.access_time),
                            title: Text('access_time')
                        )
                    ]
                )
            ```
        * ScrollDirection: 列表的滚动方向
            ```dart
                // Axis.horizontal 横向滚动
                // Axis.vertical 纵向滚动
                child: ListView(
                    scrollDirection: Axis.horizontal
                    children: <Widget>[
                        ...
                    ]
                )
            ```
        * 动态创建ListView
            ```dart
                ListView.builder(
                    itemCount: 100,
                    itemBuilder: (context, index) {
                        return ListTile(
                            title: Text('${items[index]}')
                        );
                    }
                )
            ```
    5. GridView: 网格列表组件
        * GridView.count()
        * crossAxisSpacing: 每个网格之间的间距
        * crossAxisCount: 每一行网格的数量
        * physics: NeverScrollableScrollPhysics() 禁止用户上下滚动
        ```dart
            body: GridView.count(
                padding: EdgeInsets.all(10),
                crossAxisCount: 3,
                crossAxisSpacing: 10,
                children: <Widget>[
                    Text('1'),
                    Text('1'),
                    Text('1'),
                ]
            )
        ```

        * crossAxisCount: 列数
        * mainAxisSpacing: 上下网格的间距
        * crossAxisSpacing: 左右网格的间距
        * childAspectRatio: 每个网格的宽高比（宽 : 高）
        ```dart
            body: GridView(
                gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                    crossAxisCount: 3,
                    mainAxisSpacing: 2.0,
                    crossAxisSpacing: 2.0,
                    childAspectRatio: 0.7
                ),
                children: <Widget>[
                    new Image.network('http://img5.mtime.cn/mt/2018/10/22/104316.77318635_180X260X4.jpg',fit: BoxFit.cover),
                    new Image.network('http://img5.mtime.cn/mt/2018/10/10/112514.30587089_180X260X4.jpg',fit: BoxFit.cover),
                    new Image.network('http://img5.mtime.cn/mt/2018/11/13/093605.61422332_180X260X4.jpg',fit: BoxFit.cover),
                    new Image.network('http://img5.mtime.cn/mt/2018/11/07/092515.55805319_180X260X4.jpg',fit: BoxFit.cover),
                    new Image.network('http://img5.mtime.cn/mt/2018/11/21/090246.16772408_135X190X4.jpg',fit: BoxFit.cover),
                    new Image.network('http://img5.mtime.cn/mt/2018/11/17/162028.94879602_135X190X4.jpg',fit: BoxFit.cover),
                    new Image.network('http://img5.mtime.cn/mt/2018/11/19/165350.52237320_135X190X4.jpg',fit: BoxFit.cover),
                    new Image.network('http://img5.mtime.cn/mt/2018/11/16/115256.24365160_180X260X4.jpg',fit: BoxFit.cover),
                    new Image.network('http://img5.mtime.cn/mt/2018/11/20/141608.71613590_135X190X4.jpg',fit: BoxFit.cover),
                ],
            )
        ```
    6. RaisedButton: 按钮组件
        * onPress: 点击事件
        * child
        * color
        ```dart
            child: RaisedButton(
                onPress: () {},
                color: Colors.teal,
                child: Text('button')
            )
        ```
    7. CircleAvatar: 展示头像的组件
        * backgroundImage: 背景图片
        * radius: 设置弧度， 100表圆形
        ```dart
            child: CircleAvatar(
                // 此处必须使用NetworkImage组件
                backgroundImage: NetworkImage('https://...'),
                radius: 100
            )
        ```
    8. Position: 层叠定位组件（与Stack组件合用）
        * bottom: 底部距离
        * top: 顶部距离
        * left: 左边距离 
        * right: 右边距离 
        * height: 高度 
        * width: 宽度
            ```dart
                child: Stack(
                    children: <Widget>[
                        CircelAvatar(
                            backgroundImage: NetworkImage('https://...'),
                            radius: 100
                        ),
                        Position(
                            bottom: 10,
                            right: 10,
                            child: Container(
                                child: Text('wyy')
                            )
                        )
                    ]
                )
            ``` 
    9. Divider: 分割线，可用于列表的底部边框
    10. SnackBar: 显示提示信息的控件；类比Toast
        ```dart
            Scaffold
            .of(context)
            .showSnackBar(
                SnackBar(
                    content: Text('提示信息msg;')
                )
            )
        ```
    11. BottomNavigationBar: 底部导航栏
        * 结合Scaffold组件进行使用
        * 最少有两个底部菜单按钮
        * 属性:
            * items: <`BottomNavigatorBarItem`>[]
            * currentIndex: int
            * onTap: (int index) {}
            * type: BottomNavigationBarType.fixed
            * 注意: 不同的类型切换效果会有所不同
        * BottomNavigationBarItem: 底部导航栏每一项
            * Icon
            * title
            ```dart
                import 'package:flutter/material.dart';
                import 'home.dart';
                import 'email.dart';
                import 'pages.dart';
                import 'aipPlay.dart';


                class BottomNavigatorWidget extends StatefulWidget {
                    @override
                    _BottomNavigatorWidgetState createState() => _BottomNavigatorWidgetState();
                }

                class _BottomNavigatorWidgetState extends State<BottomNavigatorWidget> {
                    final _bottomNavigatorWidgetColor = Colors.blue;
                    int _currentIndex = 0;
                    List<Widget> _list = [];

                    @override
                    void initState() { 
                        super.initState();
                        _list
                        ..add(Home())
                        ..add(Email())
                        ..add(Pages())
                        ..add(AipPlay());
                    }

                    @override
                    Widget build(BuildContext context) {
                        return Scaffold(
                            body: _list[_currentIndex],
                            bottomNavigationBar: BottomNavigationBar(
                                items: <BottomNavigationBarItem>[
                                    BottomNavigationBarItem(
                                        icon: Icon(
                                            Icons.home,
                                            color: _bottomNavigatorWidgetColor,
                                        ),
                                        title: Text(
                                            'home',
                                            style: TextStyle(color: _bottomNavigatorWidgetColor),
                                        )
                                    ),
                                    BottomNavigationBarItem(
                                        icon:Icon(
                                            Icons.email,
                                            color: _bottomNavigatorWidgetColor,
                                        ),
                                        title:Text(
                                            'Email',
                                            style:TextStyle(color: _bottomNavigatorWidgetColor)
                                        )
                                    ),
                                    BottomNavigationBarItem(
                                        icon:Icon(
                                            Icons.pages,
                                            color: _bottomNavigatorWidgetColor,
                                        ),
                                        title:Text(
                                            'Pages',
                                            style:TextStyle(color: _bottomNavigatorWidgetColor)
                                        )
                                    ),
                                    BottomNavigationBarItem(
                                        icon:Icon(
                                            Icons.airplay,
                                            color: _bottomNavigatorWidgetColor,
                                        ),
                                        title:Text(
                                            'AipPlay',
                                            style:TextStyle(color: _bottomNavigatorWidgetColor)
                                        )
                                    ),
                                ],
                                currentIndex: _currentIndex,
                                onTap: (int index) {
                                    setState(() {
                                        _currentIndex = index;
                                    });
                                },
                                type: BottomNavigationBarType.shifting,
                            ),
                        );
                    }
                }
            ```
    12. FloatingActionButton: 悬浮按钮
        * onPressed: 点击事件
        * tooltip: 长按按钮的提示信息
        * child: 放置的子元素
            ```dart
                return MaterialApp(
                    title: 'wyyApp',
                    home: Scaffold(
                        floatingActionButton: FloatingActionButton(
                            onPressed() {},
                            tooltip: '提示信息msg',
                            child: Icon(Icons.msg)
                        ),
                        // 按钮位置
                        floatingActionButtonLocation: FloatingActionButtonLocation.centerDocked
                        body: Container(...)
                    )
                )
            ```
    13. BottomAppBar: 底部工具栏
        * BottomAppBar比BottomNavigatorBar组件要灵活很多，可以放置图片、文字及容器
        * 常用属性
            * color
            * shape: 底栏的形状，设置该属性的目的是为了跟FloatingActionButton组件进行融合；大多数情况下值都为CircularNotchedRectangle()
            * child: 放置Widget,自定义底部栏的样式
                ```dart
                    bottomNavigationBar: BottomAppBar(
                        color:Colors.lightBlue,
                        shape:CircularNotchedRectangle(),
                        child: Row(
                            mainAxisSize: MainAxisSize.max,
                            mainAxisAlignment: MainAxisAlignment.spaceAround,
                            children: <Widget>[
                                IconButton(
                                    icon:Icon(Icons.home),
                                    color:Colors.white,
                                    onPressed:(){

                                    }
                                ),
                                IconButton(
                                    icon:Icon(Icons.airport_shuttle),
                                    color:Colors.white,
                                    onPressed:(){

                                    }
                                ),
                            ],
                        ),
                    )
                ```
    14. IconButton: 拥有icon属性的按钮
    15. ConstrainedBox: 约束盒子组件，添加额外的条件到child上
        ```dart
            child: ConstrainedBox(
                // 限制条件
                constraints: BoxConstraints.expand(),
                child: child
            )
        ```
    16. TabBar: tab切换组件
        * controller: 控制器，后面跟的多是TabController组件
        * tabs: <`Tab`>[] 具体切换项;
            ```dart
                bottom: TabBar(
                    controller: TabController(length:3, vsync: this),
                    tabs: <Tab>[
                        Tab(icon: Icon(Icon.add)),
                        Tab(icon: Icon(Icon.add)),
                        Tab(icon: Icon(Icon.add))
                    ]
                )
            ```
    17. TabBarView: tab切换展示视图（与TabBar合用）
        * controller: 控制器，与TabBar保持一致
        * 注意: TabBar必须结合Scaffold使用，外部一定要显式盒子高度
            ```dart 
                SizedBox(
                    height: 500,
                    child: Scaffold(
                        appBar:  AppBar( // 大量配置属性参考 SliverAppBar 示例
                        backgroundColor: Colors.amber[1000],
                        automaticallyImplyLeading: false,
                        title:  TabBar(
                            isScrollable: true,
                            controller: _tabController,
                            tabs: <Widget>[
                            Tab(text: "Tabs 1"),
                            Tab(text: "Tabs 2")
                            ],
                        ),
                        ),
                        body:  TabBarView(controller: _tabController, children: <Widget>[
                        Text('TabsView 1'),
                        Text('TabsView 2')
                        ]),
                    )

                );
            ```
        * children: 展示视图，与TabBar的选项保持一致
            ```dart
                SizedBox(
                    height: ScreenUtil.getInstance().setHeight(ScreenUtil.screenHeight),
                    child: Scaffold(
                        appBar:  AppBar(
                        backgroundColor: Colors.white,
                        automaticallyImplyLeading: false,
                        elevation: 0,
                        title:  TabBar(
                            controller: _tabController,
                            labelColor: Colors.redAccent,
                            unselectedLabelColor: Colors.black26,
                            labelStyle: TextStyle(
                            fontSize: ScreenUtil.getInstance().setSp(30)
                            ),
                            indicator: BoxDecoration(
                            border: Border(
                                bottom: BorderSide(
                                width: 1,
                                color: Colors.redAccent
                                )
                            )
                            ),
                            tabs: <Widget>[
                            Tab(
                                text: '详情',
                            ),
                            Tab(
                                text: '评论',
                            ),
                            ],
                        ),
                        ),
                        body:  TabBarView(controller: _tabController, children: <Widget>[
                        Text('TabsView 1'),
                        Text('TabsView 2')
                        ]),
                    )

                    );
            ```
    18. AppBar:     
        * title: Widget 标题
        * bottom: Widget 标题底部区域展示内容，
    19. RichText: 富文本展示
        * TextSpan
        ```dart
            title: RichText(
                text: TextSpan(
                    text: res[index].substring(0, query.length),
                    style: TextStyle(
                        color: Colors.black,
                        fontWeight: FontWeight.bold
                    ),
                    children: [
                        TextSpan(
                            text: res[index].substring(query.length),
                            style: TextStyle(
                                color: Colors.grey
                            )
                        )
                    ]
                ),
            )
        ```
    20. GestureDetector: 手势操作，用来触发事件
        * onTap: () {}
            ```dart
                child: GestureDetector(
                    onTap: () {// 触发事件},
                    child: Container(
                        child: Center(
                            child: Text('')
                        )
                    )
                )
            ```
    21. Wrap: 流式布局，类比flex-wrap: wrap;属性
        ```dart
            child: Wrap(
                children: <Widget>[],
                spacing: 20
            )
        ```
    22. Padding: 有内边距的Container容器
        ```dart
            child: Padding(
                padding: EdgeInsets.add(10),
                child: Container(
                    child: ...
                )
            )
        ```
    23. Opacity: 提供透明度选项的同容器:
        ```dart
            child: Opacity(
                opacity: .5
                child: child
            )
        ```
    24. ExpansionTile: 手风琴组件，点击再展示出来
        * title: 闭合时显示的标题，常使用Text('title')
        * leading: 标题左侧图标
        * backgroundColor: 展开时的背景颜色
        * children: 展开显示的子元素
        * trailing: 右侧的箭头
        * initiallyExpanded: 初始状态是否展开,默认为false，不展开
        ```dart
            child: Center(
                child: ExpansionTile(
                    title: Text('this is title'),
                    leading: Icon(Icons.access_time),
                    backgroundColor: Colors.white24,
                    children: <Widget>[
                        Container(
                            height: 90,
                            child: Text('lorem'),
                        )
                    ],
                    initiallyExpanded: true,
                ),
            )
        ```
    25. ExpansionPanelList: 手风琴列表组件
        * 注意: 该组件必须放到**可滑动组件**中
        * 常用属性: 
            * expansionCallback: 点击和交互的回调事件
                * 参数一: 触发动作的索引,
                * 参数二: 组件是否展开的bool值
            * children: 列表元素
    26. ExpansionPanel: 手风琴列表单项
        * headerBuilder: 标题
        * body: 展开展示的内容
        * isExpanded: 是否展开
        ```dart
            body:SingleChildScrollView(
                child: ExpansionPanelList(
                //交互回掉属性，里边是个匿名函数
                    expansionCallback: (index,bol){
                        //调用内部方法
                        _setCurrentIndex(index, bol);
                    },
                    children: mList.map((index){
                        //进行map操作，然后用toList再次组成List
                        return ExpansionPanel(
                            headerBuilder: (context,isExpanded){
                                return ListTile(
                                    title:Text('This is No. $index')
                                );
                            },
                            body:ListTile(
                                title:Text('expansion no.$index')
                            ),
                            isExpanded: expandStateList[index].isOpen
                            );
                    }).toList(),
                ),
            )
        ```
    27. SingleChildScrollView: 可滚动组件
    28. Tooltip: 包裹的元素长按会弹出提示
        ```dart
            // Tooltip包裹需要长按提示的元素
            child: Tooltip(
                message: 'msg',
                child: Container(
                    height: 100,
                    width: 100,
                    color: Colors.amber,
                ),
            ),
        ```
    29. Draggable: 定义可拖动容器
        * data: 拖动需要传递的数据
        * feedback: Widget 拖动时展示的样式组件
        * onDraggableCanceled: Function 松手时触发的回调函数
        * child: Widget 默认展示的样式
            ```dart
                child: Draggable(
                    data: widget.color, // 拖动传递的数据
                    feedback: Container( // 拖拽时展示的盒子样式
                        height: 110,
                        width: 110,
                        color: widget.color.withOpacity(.5),
                    ),
                    onDraggableCanceled: (Velocity velocity, Offset offset) { // 松手的时候触发回调
                        setState(() {
                            // 重新赋值可拖拽容器的偏移量
                            this.offset = offset;
                        });
                    },
                    child: Container(
                        height: 100,
                        width: 100,
                        color: widget.color,
                    ),
                )
            ```
    30. DragTarget: 接收拖拽结果容器
        * onAccept: Function 接收拖拽结果触发回调，其参数为Draggalbe中data属性值
        * builder: Function 返回默认展示样式
            ```dart
                DragTarget(
                    onAccept: (Color color) {
                        // 接收拖拽结果，触发更新回调
                        setState(() {
                            _color = color;
                        });
                    },
                    builder: (context, cadi, reje) {
                        return Container(
                            height: 200,
                            width: 200,
                            color: _color,
                        );
                    },
                ),
            ```
    31. TextField: 文本输入框
        * controller: TextEditingController 控制器（获取用户输入）
        * decoration: 装饰器
        * autofocus: 是否自动聚焦
            ```dart
                TextEditingController _textEditingController = TextEditingController();
                child: TextField(
                    controller: _textEditingController,
                    decoration: InputDecoration(
                        labelText: '',
                        helperText: '',
                        contentPadding: EdgeInsert.all(10)
                    ),
                    autofocus: false
                )
                // _textEditingController.text可获取用户的输入值
            ```
    32. InkWell: 水波纹组件
        * 可以作用在Container、Image等各种组件，不仅仅是按钮
            ```dart
                child: InkWell(
                    onTap: (){},
                    child: Container(
                        child: ...
                    )
                )
            ```
    33. Border: 设置边框
        ```dart
            decoration: BoxDecoration(
                color: Colors.white,
                border: Border(
                    bottom: BorderSide(width: .5, color: Colors.black12)
                )
            )
        ```
    34. ClipOval: 原型裁切组件
    35. BorderRadius: 设置圆角
        ```dart
            return new Center(
                child: Container(
                    width: 300,
                    height: 300,
                    decoration: BoxDecoration(
                    color: Colors.yellow,
                    image: DecorationImage(
                        image: AssetImage(R.assetsImg1),
                        fit: BoxFit.cover),
                        // 设置圆角
                        borderRadius: BorderRadius.circular(150)
                    ),
            ),
        ```
    36. DecorationImage: 背景图片设置
        ```dart
            Container(
                width: ScreenUtil.getInstance().setWidth(750),
                height: ScreenUtil.getInstance().setHeight(400),
                alignment: Alignment.center,
                decoration: BoxDecoration(
                    image: DecorationImage(
                        image: NetworkImage('')
                    ),
                ),
        ```
    37. ListTile.divideTiles: 添加底部边框分割线
        ```dart
            
            ListView(
                children: ListTile.divideTiles(
                    context: context,
                    tiles: [
                        ListTile(
                        title: Text('Horse'),
                        ),
                        ListTile(
                        title: Text('Cow'),
                        ),
                        ListTile(
                        title: Text('Camel'),
                        ),
                        ListTile(
                        title: Text('Sheep'),
                        ),
                        ListTile(
                        title: Text('Goat'),
                        ),
                    ]
                ).toList(),

        ```
    38. Form: 表单组件
        ```dart
            import 'package:flutter/material.dart';

            void main() => runApp(MyApp());

            // 这个 widget 作用这个应用的顶层 widget.
            //这个 widget 是无状态的，所以我们继承的是 [StatelessWidget].
            //对应的，有状态的 widget 可以继承 [StatefulWidget]
            class MyApp extends StatelessWidget {

            @override
            Widget build(BuildContext context) {
                // 我们想使用 material 风格的应用，所以这里用 MaterialApp
                return MaterialApp(
                // 移动设备使用这个 title 来表示我们的应用。具体一点说，在 Android 设备里，我们点击
                // recent 按钮打开最近应用列表的时候，显示的就是这个 title。
                title: 'Welcome to Flutter',
                // 应用的“主页”
                home: LoginPage(),
                );
            }
            }

            class LoginPage extends StatefulWidget {
            @override
            _LoginPageState createState() => _LoginPageState();
            }

            class _LoginPageState extends State<LoginPage> {
            //全局 Key 用来获取 Form 表单组件
            GlobalKey<FormState> loginKey = GlobalKey<FormState>();

            //用户名
            String userName;

            //密码
            String password;

            void login() {
                //读取当前 Form 状态
                var loginForm = loginKey.currentState;
                //验证 Form表单
                if (loginForm.validate()) {
                loginForm.save();
                print('userName：' + userName + '，password：' + password);
                }
            }

            @override
            Widget build(BuildContext context) {
                return Scaffold(
                appBar: AppBar(
                    title: Text('Form 表单组件'),
                    centerTitle: true,
                ),
                body: Column(
                    children: <Widget>[
                    Container(
                        padding: EdgeInsets.all(16),
                        child: Form(
                        //设置globalKey，用于后面获取FormState
                        key: loginKey,
                        //开启自动校验
                        autovalidate: true,
                        child: Column(
                            children: <Widget>[
                            TextFormField(
                                decoration: InputDecoration(
                                labelText: '请输入用户名',
                                hintText: "用户名或邮箱",
                                hintStyle: TextStyle(
                                    color: Colors.grey,
                                    fontSize: 13,
                                ),
                                prefixIcon: Icon(Icons.person),
                                ),
                                //校验用户
                                validator: (value) {
                                return value.trim().length > 0 ? null : "用户名不能为空";
                                },
                                //当 Form 表单调用保存方法 Save时回调的函数。
                                onSaved: (value) {
                                userName = value;
                                },
                                // 当用户确定已经完成编辑时触发
                                onFieldSubmitted: (value) {},
                            ),
                            TextFormField(
                                decoration: InputDecoration(
                                labelText: '请输入密码',
                                hintText: '你的登录密码',
                                hintStyle: TextStyle(
                                    color: Colors.grey,
                                    fontSize: 13,
                                ),
                                prefixIcon: Icon(Icons.lock),
                                ),
                                //是否是密码
                                obscureText: true,
                                //校验密码
                                validator: (value) {
                                return value.length < 6 ? '密码长度不够 6 位' : null;
                                },
                                onSaved: (value) {
                                password = value;
                                },
                            )
                            ],
                        ),
                        //内容改变的回调
                        onChanged: () {
                            print("onChanged");
                        },
                        ),
                    ),
                    Container(
                        padding: EdgeInsets.all(16),
                        child: Row(
                        children: <Widget>[
                            Expanded(
                            child: RaisedButton(
                                padding: EdgeInsets.all(15),
                                child: Text(
                                "登录",
                                style: TextStyle(fontSize: 18),
                                ),
                                textColor: Colors.white,
                                color: Theme.of(context).primaryColor,
                                onPressed: login,
                            ),
                            ),
                        ],
                        ),
                    )
                    ],
                ),
                );
            }
            }
        ```
    39. Align: 父组件宽高由子组件宽高决定
        ```dart
            // 该盒子宽高由盒子2的宽高决定: 20 * 3
            Container( // 盒子1 
                color: Colors.redAccent,
                child: Align(
                    heightFactor: 3, // 高度因子
                    widthFactor: 3, // 宽度因子
                    // Center组件也可控制宽高因子，从而控制父盒子的宽高
                    alignment: Alignment.topCenter,
                    
                    child: Container( // 盒子2
                        color: Colors.black26,
                        width: 20,
                        height: 20,
                    ),
            )
        ```
3. class类中接收参数
    * `final List<String> items;`
    * `MyApp({Key key, @required this.items}):super(key: key)`

4. flutter中的布局
    1. Row: 水平布局
        * 非灵活排列: **根据子元素大小进行布局（保持子元素的真实大小），子元素不足会留出空隙，超出会出现警告**
        * 灵活排列: **子元素会充满一行，类比flex-grow: 1**
            * Expanded嵌套每一个子元素
            ```dart
                child: Row(
                    children: <Widget>[
                        // button组件默认会充满一整行
                        Expanded(
                            child: RaisedButton(
                                onPress: () {},
                                child: Text('button')
                            )
                        )
                    ]
                )
            ```
        * 非灵活排列跟灵活排列也可一块混用
            ```dart
                child: Row(
                    children: <Widget>[
                        // 两边的button为非灵活排列，会保持元素的真实大小
                        RaisedButton(
                            child: Text('button'),
                            onPress: () {}
                        ),
                        // 中间的button采用了灵活排列，会充满剩余空间
                        Expanded(
                            child: RaisedButton(
                                child: Text('button'),
                                onPress: () {}
                            ),
                        ),
                        RaisedButton(
                            child: Text('button'),
                            onPress: () {}
                        ),
                    ]
                )
            ```
    2. Column: 锤子布局
        * CrossAxisAlignment: 副轴对齐方式
            * CrossAxisAlignment.start 左对齐
            * CrossAxisAlignment.end 右对齐
            * CrossAxisAlignment.center 居中对齐
            ```
                body: Column(
                    children: <Widget>[
                        crossAxisAlignment: CrossAxisAlignment.start
                    ]
                )
            ```
        * MainAxisAlignment: 主轴的对齐方式
        * **注意: 主轴和副轴是相对而言，Row组件的主轴是水平方向，Column组件的主轴方向是垂直方向**
    3. Stack: 层叠布局(类比定位)
        * alignment: 控制层叠的位置，两个值分贝代表x轴和y轴，0 ~ 1
            ```dart
                child: Stack(
                    // 居中
                    alignment: FractionalOffset(0.5, 0.5),
                    children: <Widget>[
                        CircleAvatar(
                            backgroundImage: NetworkImage('https://...'),
                            radius: 100
                        ),
                        Container(
                            child: Text('...')
                        ),
                    ]
                )
            ```
        * Position: 定位组件
    4. Card: 卡片式布局(外部有阴影)
        ```dart
            // Card组件会默认充满整个屏幕
            child: Card(
                child: Column(
                    children: <Widget>[
                        ...
                    ]
                )
            )
        ```

5. flutter中的导航
    1. Navigator: 跳转，类比history
        * Navigator.push: 跳转到下一页面
            * 参数一: context 上下文
            * 参数二: callback 跳转回调函数
        * Navigator.pop: 返回上一个页面
            * context: 上下文
            * **注意: 页面必须在调用过Navigator.push后，才能调用Navigator.pop**
        ```dart
            import 'package:flutter/material.dart';

            void main() => runApp(
                // 注意: 需将MaterialApp组件提前
                MaterialApp(
                    title: 'wyy',
                    home: Home(),
                )
            );

            class Home extends StatelessWidget {

                @override
                Widget build(BuildContext context) {
                    return Scaffold(
                        appBar: AppBar(
                            title: Text('Home'),
                        ),
                        body: Center(
                                child: RaisedButton(
                                color: Colors.amber,
                                child: Text('go detail'),
                                onPressed: () {
                                    Navigator.push(context, MaterialPageRoute(
                                        builder: (context) => Detail()
                                    ));
                                },
                            ),
                        ),
                    );
                }
            }

            class Detail extends StatelessWidget {

                @override
                Widget build(BuildContext context) {
                    return Scaffold(
                        appBar: AppBar(
                            title: Text('Detail'),
                        ),
                        body: Center(
                                child: RaisedButton(
                                child: Text('back Home'),
                                onPressed: () {
                                    Navigator.pop(context);
                                },
                            ),
                        ),
                    );
                }
            }
        ```
    2. 参数传递及接收
        ```dart
            class Home extends StatelessWidget {
                // 声明类属性,需要在创建该实例时传入该参数
                final List<String> items;
                // 接收参数
                Home({Key key, @required this.items}):spuer(key: key);
                @override
                Widget build(BuildContext context) {
                    return Scaffold(
                        ...
                    )
                }
            }
        ```   
    3. 返回结果到上一页数据(async、await)

            ```dart

                // 定义异步方法
                _navigatorGoSonPage(BuildContext context) async {
                    // await得到子页面传递的数据 
                    final res = await Navigator.push(
                        context,
                        MaterialPageRoute(
                            builder: (context) => SonPage()
                        )
                    )
                    Scaffold.of(context).showSnackBar(
                        SnackBar(
                            content: Text('得到的数据为: $res')
                        )   
                    )
                }

                // 子页面传递数据
                Navigator.pop(context, '需要传递到上一页的数据')
            ```
    4. 静态资源处理(pubspec.yaml)
        * 引入的静态资源文件需要在pubspec.yaml中声明
            * yaml中声明静态文件
            ```yaml
                # 表明项目根目录文件下: images文件中的demo.png
                # 注意: 是项目根目录，而不是lib目录文件夹
                assets:
                    - images/demo.png
            ```
            * dart中使用静态文件
            ```dart
                child: Container(
                    child: Image.asset('images/demo.png')
                )
            ```
    5. 自定义主题theme
        ```dart
            return MaterialApp(
                title: 'wyyApp',
                theme: ThemeData(
                    primarySwatch: Colors.lightBlue
                ),
                home: Container(...)
            )
        ```
6. flutter小实战
    1. 底部导航栏切换(bottomNavigatorBar: BottomNavigatorBar())
        ```dart
            class HomeWidgetState extends State<Home> {
                // 当前页
                int _currentIndex = 0;
                // BottomNavigatorBar列表页
                List<Widget> list = [];

                @override
                void initState() {
                    super.initState();
                    // 填充每一页数据
                    list
                        ..add(Home())
                        ..add(Email())
                        ..add(Pages());
                }

                @overrid
                Widget build(BuildContext context) {
                    return Scaffold(
                        body: list[_currentIndex],
                        bottomNavigatorBar: BottomNavigatorBar(
                            items: <BottomNavigatorBarItem>[],
                            currentIndex: _currentIndex,
                            onTap: (int Index) {
                                setState(() {
                                    // 数据切换，触发更新回调
                                    _currentIndex = index;
                                });
                            },
                            type: BottomNavigatorBarType.shifting
                        )
                    )
                }
            } 
        ```
    2. 不规则底部导航栏(bottomNavigatorBar: BottomAppBar())
        * BottomAppBar + FloatingActionBar
        ```dart
            import 'package:flutter/material.dart';
            import 'eachView.dart';

            class BottomBar extends StatefulWidget {
                BottomBar({Key key}) : super(key: key);

                @override
                _BottomBarState createState() => _BottomBarState();
            }

            class _BottomBarState extends State<BottomBar> {
                List<Widget> list = [];
                int _currentIndex = 0;

                @override
                void initState() { 
                    super.initState();
                    list = [];
                    list
                        ..add(EachView(title: 'Home'))
                        ..add(EachView(title: 'AirPlanet'));
                }

                @override
                Widget build(BuildContext context) {
                    return Scaffold(
                        body: list[_currentIndex],
                        floatingActionButton: FloatingActionButton(
                            child: Icon(
                                Icons.add,
                                color: Colors.white,
                            ),
                            tooltip: 'msg',
                            onPressed: () {
                                Navigator.push(context, MaterialPageRoute(
                                    builder: (context) {
                                        return EachView(title: 'NewPage');
                                    }
                                ));
                            },
                        ),
                        floatingActionButtonLocation: FloatingActionButtonLocation.centerDocked,
                        bottomNavigationBar: BottomAppBar(
                            color: Colors.lightBlue,
                            shape: CircularNotchedRectangle(),
                            child: Row(
                                mainAxisSize: MainAxisSize.max,
                                mainAxisAlignment: MainAxisAlignment.spaceAround,
                                children: <Widget>[
                                    IconButton(
                                    icon: Icon(Icons.home, color: Colors.white),
                                    onPressed: (){
                                        setState(() {
                                            _currentIndex = 0;
                                        });
                                    },
                                    ),
                                    IconButton(
                                    icon: Icon(Icons.airport_shuttle, color: Colors.white),
                                    onPressed: () {
                                        setState(() {
                                        _currentIndex = 1;
                                        });
                                    },
                                    )
                                ],
                            ),
                        ),
                    );
                }
            }
        ```
    3. 自定义路由跳转动画(继承PageRouteBuilder，并重新实现transitionsBuilder方法)   
        ```dart
            import 'package:flutter/material.dart';
            class CustomRoute extends PageRouteBuilder{
                final Widget widget;
                CustomRoute(this.widget)
                    :super(
                        transitionDuration:const Duration(seconds:1),
                        pageBuilder:(
                            BuildContext context,
                            Animation<double> animation1,
                            Animation<double> animation2){
                            return widget;
                            },
                        transitionsBuilder:(
                            BuildContext context,
                            Animation<double> animation1,
                            Animation<double> animation2,
                            Widget child){
                                // 重写路由动画
                                return FadeTransition(
                                    opacity: Tween(begin:0.0,end :1.0).animate(CurvedAnimation(
                                        parent:animation1,
                                        curve:Curves.fastOutSlowIn
                                    )),
                                    child: child,
                                );
                            }  
                    ); 
            }
        ```
        * 使用: 
            * Navigator.push(context, CustomRoute(Page()));
            * 代替使用: Navigator.push(context, MaterialPageRoute(builder: (context) => Page()));
        * 渐隐渐现
            ```dart
                FadeTransition(
                    opacity: Tween(begin:0.0,end :1.0).animate(CurvedAnimation(
                        parent:animation1,
                        curve:Curves.fastOutSlowIn
                    )),
                    child: child,
                );
            ```
        * 旋转加缩放
            ```dart
                RotationTransition(
                    turns:Tween(begin:0.0,end:1.0)
                    .animate(CurvedAnimation(
                        parent: animation1,
                        curve: Curves.fastOutSlowIn
                    )),
                    child:ScaleTransition(
                        scale:Tween(begin: 0.0,end:1.0)
                        .animate(CurvedAnimation(
                            parent: animation1,
                            curve:Curves.fastOutSlowIn
                        )),
                        child: child,
                    )
                );
            ```
        * 左右滑动
            ```dart
                SlideTransition(
                    position: Tween<Offset>(
                        begin: Offset(-1.0, 0.0),
                        end:Offset(0.0, 0.0)
                    )
                    .animate(CurvedAnimation(
                        parent: animation1,
                        curve: Curves.fastOutSlowIn
                    )),
                    child: child,
                );

            ```


    4. 保持页面的状态: AutomaticKeepAliveClientMixin
        * 使用该mixin必须是StatefulWidget
        * 重写wantKeepAlive方法
        * 只有PageView和IndexedStack包裹组件才能保持页面状态
            * PageView => PageController
            * [简书地址](https://www.jianshu.com/p/86d29a939624)
            * 最简单的实现步骤:
                * body返回IndexedStack()即可
    5. 联动搜索: 
        * 在按钮事件中直接调用: showSearch(context: context, delegate: MySearch());该方法由父类组件StateFulWidget进行提供
        ```dart
            class MySearch extends SearchDelegate {
                // 点击搜索框右侧的clear按钮
                @override
                List<Widget> buildActions(BuildContext context) {
                    return [
                    IconButton(
                        icon: Icon(Icons.clear),
                        // query表示搜索的内容，继承自父类的属性
                        onPressed: () => query = '',
                    )
                    ];
                }

                // 点击左侧返回按钮事件
                @override
                Widget buildLeading(BuildContext context) {
                    return IconButton(
                    icon: AnimatedIcon(
                        icon: AnimatedIcons.menu_arrow,
                        progress: transitionAnimation,
                    ),
                    onPressed: () => close(context, null),
                    );
                }

                // 展示结果
                @override
                Widget buildResults(BuildContext context) {
                    return Container(
                    width: 100,
                    height: 100,
                    child: Card(
                        color: Colors.amber,
                        child: Center(
                        child: Text(query),
                        ),
                    ),
                    );
                }

                // 联动搜索提示信息
                @override
                Widget buildSuggestions(BuildContext context) {
                    final res = query.isEmpty ? recentSuggest : searchList.where((input) => input.startsWith(query)).toList();
                    return ListView.builder(
                    itemCount: res.length,
                    itemBuilder: (context, index) {
                        return ListTile(
                            title: RichText(
                                text: TextSpan(
                                    text: res[index].substring(0, query.length),
                                    style: TextStyle(
                                        color: Colors.black,
                                        fontWeight: FontWeight.bold
                                    ),
                                    children: [
                                        TextSpan(
                                            text: res[index].substring(query.length),
                                            style: TextStyle(
                                                color: Colors.grey
                                            )
                                        )
                                    ]
                                ),
                            ),
                        );
                    },
                    );
                }
            }
        ```
    6. 流式布局: 
        * 屏幕宽高的获取:
            * MediaQuery.of(context).size.width
            * MediaQuery.of(context).size.height
    7. 闪屏动画的制作:
        ```dart
            import 'package:flutter/material.dart';
            import 'home.dart';

            class SplashDemo extends StatefulWidget {
            SplashDemo({Key key}) : super(key: key);

            @override
            _SplashDemoState createState() => _SplashDemoState();
            }

            class _SplashDemoState extends State<SplashDemo> with SingleTickerProviderStateMixin {

            AnimationController _animationController;
            Animation _animation;

            @override
            void initState() { 
                super.initState();
                _animationController = AnimationController(
                    vsync: this, duration: Duration(milliseconds: 3000)
                );
                _animation = Tween(begin: 0, end: 1).animate(_animationController);
                // 监听动画的状态
                _animation.addStatusListener((status) {
                    if(status == AnimationStatus.completed) {
                        // 跳转路由，并结束当前动画，节约性能
                        Navigator.of(context).pushAndRemoveUntil(
                            MaterialPageRoute(
                                builder: (context) => Home()
                            ), 
                            (route) => route == null
                        );
                    }
                });

                _animationController.forward(); //播放动画
            }

            @override
            void dispose() {
                super.dispose();
                _animationController.dispose();    
            }

            @override
            Widget build(BuildContext context) {
                // 渐显动画
                return FadeTransition(
                opacity: _animationController,
                child: Image.network('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577153300127&di=aa094af081c989e0dbd6271873d9dcc9&imgtype=0&src=http%3A%2F%2Fd.paper.i4.cn%2Fmax%2F2017%2F01%2F09%2F16%2F1483950666417_942143.jpg',
                    fit: BoxFit.cover,
                    scale: 2.0,
                ),
                
                );
            }
            }
        ```
    8. 右划返回上一页: 
        * cupertino.dart主题自带,用CupertinoPageRoute进行路由跳转，即可实现
            ```dart
                import 'package:flutter/cupertino.dart';

                class RightBackDemo extends StatelessWidget {
                const RightBackDemo({Key key}) : super(key: key);

                @override
                Widget build(BuildContext context) {
                    return CupertinoPageScaffold(
                    child: Center(
                            child: Container(
                            height: 100,
                            width: 100,
                            color: CupertinoColors.activeBlue,
                            child: CupertinoButton(
                                onPressed: () {
                                Navigator.of(context).push(
                                    CupertinoPageRoute(
                                    builder: (context) => RightBackDemo()
                                    )
                                );
                                },
                                child: Icon(CupertinoIcons.add),
                            ),
                            ),
                        ),
                    );
                }
                }
            ```
    9. 拖拽效果:
        ```dart
            import 'package:flutter/material.dart';

            class DargWidget extends StatefulWidget {
            final Offset offset;
            final Color color;
            DargWidget({Key key, @required this.offset, @required this.color}) : super(key: key);

            @override
            _DargWidgetState createState() => _DargWidgetState();
            }

            class _DargWidgetState extends State<DargWidget> {
            Offset offset = Offset(0, 0);

            @override
            void initState() { 
                super.initState();
                offset = widget.offset;
            }


            @override
            Widget build(BuildContext context) {
                return Positioned(
                left: offset.dx,
                top: offset.dy,
                child: Draggable(
                    data: widget.color, // 拖动传递的数据
                    feedback: Container( // 拖拽时展示的盒子样式
                    height: 110,
                    width: 110,
                    color: widget.color.withOpacity(.5),
                    ),
                    onDraggableCanceled: (Velocity velocity, Offset offset) { // 松手的时候触发回调
                    setState(() {
                        // 重新赋值可拖拽容器的偏移量
                        this.offset = offset;
                    });
                    },
                    child: Container(
                    height: 100,
                    width: 100,
                    color: widget.color,
                    ),
                )
                );
            }
            }
        ```

        ```dart
            import 'package:flutter/material.dart';
            import 'dragWidget.dart';

            class DragDemo extends StatefulWidget {
            DragDemo({Key key}) : super(key: key);

            @override
            _DragDemoState createState() => _DragDemoState();
            }

            class _DragDemoState extends State<DragDemo> {
            Color _color = Colors.grey;

            @override
            Widget build(BuildContext context) {
                return Scaffold(
                body: Stack(
                    children: <Widget>[
                    DargWidget(
                        offset: Offset(80, 80),
                        color: Colors.tealAccent,
                    ),
                    DargWidget(
                        offset: Offset(180, 80),
                        color: Colors.redAccent,
                    ),
                    Center(
                        child: DragTarget(
                        onAccept: (Color color) {
                            setState(() {
                            _color = color;
                            });
                        },
                        builder: (context, cadi, reje) {
                            return Container(
                            height: 200,
                            width: 200,
                            color: _color,
                            );
                        },
                        ),
                    )
                    ],
                ),
                );
            }
            }
        ```
7. with: 混入
    1. SingleTickerProviderStateMixin: TabBar(TabController中的vsync属性)
    2. AutomaticKeepAliveClientMixin: keepAlive(保持页面的状态)
        ```dart
            class Home extends StatefulWidget {
                _HomeState createState() => _HomeSatte();
            }
            // 混入，表明该组件在页面切换时会保存当前的状态
            class _HomeState extends State<Home> with AutomaticKeepAliveClientMixin {

                // 重写wantKeepAlive方法
                @override
                bool get wantKeepAlive => true;

                @override
                Widget build(BuildContext context) {
                    return Scaffold(
                        body: ...
                    );
                }

            }
        ```
