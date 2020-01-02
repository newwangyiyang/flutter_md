### 常用库
1. dio: dart中的第三方http请求库
    * [git地址](https://github.com/flutterchina/dio/blob/master/README-ZH.md)
    * 命令: flutter packages get 加载第三包
    * flutter中引入第三方库: 
        ```yaml
            // pubspec.yaml
            dependencies:
                dio: ^3.0.7
        ```
2. flutter_swiper: 轮播图组件
    * [git 地址](https://github.com/best-flutter/flutter_swiper/blob/master/README-ZH.md)
    * 注意: **外层嵌套的Container容器别忘记设置高度**
        ```yaml
            flutter_swiper: ^1.1.6
        ```
3. flutter_screenutil: 屏幕适配方案
    * [git地址](https://github.com/OpenFlutter/flutter_screenutil/blob/master/README_CN.md)
    * 常用API:
        * 设置宽高: ScreenUtil().setHeight(333)
        * 设置字体大小: ScreenUtil.getInstance().setSp(28)  
        * ScreenUtil.pixelRatio : 设备的像素密度
        * ScreenUtil.screenWidth : 设备的宽度
        * ScreenUtil.screenHeight : 设备高度
        ```yaml
            flutter_screenutil: ^0.7.0
        ```
4. url_launcher: 可以打开网页，发送邮件，还可以拨打电话。
    * [git地址](https://github.com/flutter/plugins/tree/master/packages/url_launcher/url_launcher)
5. flutter_easyrefresh: 上拉加载更多，下拉刷新
    * [git地址](https://github.com/xuelongqy/flutter_easyrefresh)
6. 状态管理工具: provide
    * [git地址](https://github.com/google/flutter-provide)
    * 顶层依赖声明:
        ```dart
            void main() => 
                runApp(
                    // 顶层方法中声明Provides
                    // 如果有多个依赖直接通过..语法进行链式编程添加
                    ProviderNode(
                        providers: Providers()..provide(Provider.function((context) => Counter())),
                    child: MyApp(),
                    )
                );
        ```
    * 使用状态和方法:
        * 状态值:
            ```dart
                child: Provide<Counter>(
                    builder: (context, child, counter) => Text('${counter.val}')
                )
            ```
        * 方法
            ```dart
                final currentCount = Provide.value<Counter>(cntext);

                onPress: () {
                    currentCount.changeVal();
                }
            ```

    ```yaml
        provide: ^1.0.2
    ```
7. Flutter Loading插件: flutter_easyloading
    * [git地址](https://github.com/huangjianke/flutter_easyloading/blob/master/README-zh_CN.md)
8. Flutter Toast插件: fluttertoast
    * [git地址](https://github.com/PonnamKarthik/FlutterToast)
9. 路由: fluro: flutter中最成熟的路由框架
    * [git地址](https://github.com/theyakka/fluro)
    * **将路由单独抽离出来**
        * routers文件夹:
            * router_handler.dart : 构造路由规则处理函数（传递参数）
            * routes.dart : 路由配置
    * 使用步骤:
        1. 创建Router实例: final router = Router();
        2. 声明路由处理函数: 
            ```dart
                Handler detailsPageHandler = Handler(
                    handlerFunc: (BuildContext context, Map<String, dynamic> params) {
                        // id为DetailsPage需要接收到的参数
                        return DetailsPage(id: params['id'][0])
                    }
                );
            ```
        3. 路由整体配置: 路由路径、根目录、define配置
            ```dart
                class Routes {
                    static String root = '/';
                    static String detailPage = '/detailPage/:id';
                    static void configRoutes(Router router) {
                        router.notFoundHandler = Handler( // 未找到路由的配置，可跳转到固定页面，或进行相应提示
                        handlerFunc: (BuildContext context, Map<String, dynamic> params) {
                            EasyLoading.showError('未找到对应页面');
                        }
                        );

                        // 配置路由（很重要）
                        // 将路径跟handler一一对应，没一个路径对应一个handler
                        router..define(detailPage, handler: detailHandler);
                        
                    }
                }
            ```
        4. 将路由注入到程序中:
            ```dart
                class MyApp extends StatelessWidget {
                    @override
                    Widget build(BuildContext context) {
                        //-------------------主要代码start
                        final router = Router();
                        Routes.configureRoutes(router);
                        Application.router=router;
                        //-------------------主要代码end


                        return Container(

                            child: MaterialApp(
                                title:'百姓生活+',
                                debugShowCheckedModeBanner: false,
                                //----------------主要代码start
                                onGenerateRoute: Application.router.generator,
                                //----------------主要代码end
                                theme: ThemeData(
                                primaryColor:Colors.pink,
                                ),
                                home:IndexPage()
                            ),
                            );
                        }
                    }
            ```
        5. 最后进行路由的跳转:
            ```dart
                 Application.router.navigateTo(context,"/detail/${val['goodsId']}");
            ```
    * 页面返回仍是用: Navigator.pop(context);
    ```yaml
        fluro: "^1.5.1"
    ```
10. flutter_html: 解析静态html标签的Flutter Widget
    * [git地址](https://github.com/Sub6Resources/flutter_html)
    ```dart
        return Container(
          // 将html标签渲染成为Widget
          child: Html(
            data: dgp.detailGoodsDto.goodInfo.goodsDetail,
          ),
        );
    ```
11. shared_preferences: Flutter官方插件，利用key-value的形式进行APP端的数据持久化
    * [git地址](https://github.com/flutter/plugins/tree/master/packages/shared_preferences/shared_preferences)
    * 利用key-value的形式对数据进行持久化
        ```dart
            SharedPreferences prefs = await SharedPreferences.getInstance();
            // 新增、获取、删除
            prefs.setStringList(key, [value]),
            prefs.getStringList(key)
            prefs.remove(key)
        ```
    * 注意: 只支持字符串的数据持久化，不支持对象，
        * 需利用json.encode()将对象转为字符串
12. permission_handler: 权限相关插件
    * [git地址](https://github.com/Baseflow/flutter-permission-handler)
13. amap_map_fluttify: 高德地图
    * [git地址](https://github.com/fluttify-project/amap_map_fluttify)
### 开发技巧
1. JSON数据解析:
    1. json.decode(): 将String字符串转换为Map
        * 适用于不复杂的json数据结构
    2. 解析成对象，bean实例
        ```dart
            class Demo {
                String name;
                int age;
                Demo({this.name, this.gae});
                // 工厂模式进行构造实例
                factory Demo.fromJson(Map map) {
                    return Demo(
                        name: map['name'],
                        age: map['age']
                    )
                }
            }
        ```
2. FutureBuilder(常用): 组件，等待异步请求，解决了异步渲染的问题
    * 不用setState去改变状态(接收到接口返回结果后自动改变)
    * future: 请求的接口
    * builder: 构造样式组件
        ```dart
            body:FutureBuilder(
                // 调用请求数据
                future:getHomePageContent(),
                builder: (context,snapshot){
                    // snapshot中判断接口是否请求成功，并获取到数据
                    if(snapshot.hasData){
                        var data=json.decode(snapshot.data.toString());
                        List<Map> swiperDataList = (data['data']['slides'] as List).cast(); // 顶部轮播组件数
                        return Column(
                        children: <Widget>[
                                SwiperDiy(swiperDataList:swiperDataList ),   //页面顶部轮播组件
                            ],
                        );
                    }else{
                        // 接口请求中页面展示内容
                        return Center(
                            child: Text('加载中'),
                        );
                    }
                },
            )
        ```
3. 类型强转: as
    * 常用于json.decode()之后，数据强转
    ```dart
        var data = json.decode(response);
        List<Map> list = (data['list'] as List).cast()
        // as List 强转为列表List类型
        // .cast() 将list中每一项转换为Map类型
    ```
4. context: 作用域上下文，在继承自StatefulWidget的子类中可全局获取
    * 继承自StatelessWidget类，则需要进行参数传递获取，不能全局获取
    ```dart
        class Demo extends StatefulWidget {
            ...

            void _changeSomething() {
                // context可在类中任何地方进行获取
                Provide.value<DemoProvide>(context).changeSomething();
            }
        }
    ```
5. List跟List<Map>是有区别的,一般声明List如果存储类型不确定，直接使用List声明就好
    ```dart
        List list = json.decode(res)['data']

        List<Map> list = json.decode(res)['data'] // 此种方式会报错
    ```
6. Model层转List技巧
    ```dart
        List<Demo> list = res.map((item) => Demo.fromJaon(item)).toList();
    ```
7. Expanded: 高度自适应, 可使用Expanded容器自动填充剩余空间（对自使用布局很有效）
8. ScrollController: 返回顶部，用于ListView中，控制列表的滚动举例
    ```dart
        child:ListView.builder(
            controller: scrollController,
            itemCount: data.goodsList.length,
            itemBuilder: (context,index){
                return _ListWidget(data.goodsList,index);
            },
        ),
        // 滚动到首页
        // 注意1: 由于首次进入页面无法直接滚动到顶部，会抛异常，可使用try...catch进行捕获
        // 注意2: 切换tab滚动到顶部，该代码一般会放在builder回到函数中
        ListView.builder(
            controller: scrollController,
            itemCount: 10,
            itemBuilder: (context, index) {
                try{
                    scrollController.jumpTo(0);
                }catch(e) {
                    print('用户首页进入页面，滚动到顶部会抛异常: $e');
                }
                // 列表项
                return _ListWidget(...)
            }
        )
        
    ```
9. Column中元素过多，造成溢出问题，使用ListView进行代替
    ```dart
        // ListView允许用户进行滑动
        child: ListView(
            children: <Widget>[
                // 很多个Widget
            ]
        )
    ```
10. List中Api:
    * where => 类比于js的filter
    * reduce => 无法设置初始值
        * fold => 可设置初始值，类比于js中的reduce
            ```dart
                list.fold(0, (a, b) => a + b.count * b.price);
            ```
11. StatefulWidget的生命周期
    * StatefulWidget本身由两个类组成: StatefulaWidget和State组成
        * 首先执行StatefulWidget中的生命周期:
            1. 执行StatefulWidget的构造函数(Constructor)来创建出StatefulWidget
            2. 执行StatefulWidget的createState方法，来创建一个维护StatefulWidget的State对象
        * 然后，调用createState创建的State对象，执行State类的相关方法
            1. 执行构造方法(Constructor)，创建State对象
            2. initState: 初始化数据，例如发送网络请求;在整个生命周期中只调用一次
            3. didChangeDependencies: 
                * 紧跟initState后调用
                * 从其他对象中依赖一些数据发生改变时
            4. build: 创建需要渲染的Widget
            5. deactivate: 组件销毁前进行调用
            6. dispose: 销毁组件时调用
            7. didUpdateWidget: Widget 的配置发生变化时，或热重载时，系统会回调该方法,很少用到
        * 手动调用setState方法，会根据最新状态，重新调用build方法，构建对应的Widget
