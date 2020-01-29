# angular8
# 脚手架安装
1. npm i -g @angular/cli
2. 检查： ng v

# 项目创建
1. ng new 项目名称
2. ng serve 运行项目

# vscode angular提示工具
1. angular snippet

# 项目目录
1. e2e端到端测试
2. src编写code的地方

# 模块
```js
import { BrowserModule } from '@angular/platform-browser';
// 核心模块
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
// 根组件
import { AppComponent } from './app.component';
// 装饰器
@NgModule({
  // 配置当前项目运行的组件
  declarations: [
    AppComponent
  ],
  // 项目所需要的依赖
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  // 配置项目所需要的服务
  providers: [],
  // 指定应用的主视图
  bootstrap: [AppComponent]
})
// 根模块不需要让其他模块引入
export class AppModule { }
```

# 创建组件
1. ng g c 组件名称
2. ng g c components/news // 会在app下面新建一个components文件夹，下面再新建一个news文件夹
3. 每一个页面可以看作一个组件，需要注册到app.module.ts中

# 绑定数据
1. {{}}
2. angular是基于ts的，定义数据的时候建议指定类型
3. <div [title]="变量"></div>

# 绑定html
1. <span>[innerHTML]="'<h1></h1>'"</span>

# 数据循环
```js
<ul>
  <li *ngFor="let item of list;let key = index;">{{ item }}</li>
</ul>
```

# 条件判断
1. *ngIf
2. *ngSwitch
```html
<span *ngSwitch="变量名">
  <p *ngSwitchCase="1">
    测试switch1
  </p>
  <p *ngSwitchCase="2">
    测试switch2
  </p>
  <p *ngSwitchDefault="3">
    测试switch3
  </p>
</span>
```

# [ngClass]
```html
<div [ngClass]="{'red': true} | []">

</div>
```

# [ngStyle]
```html
<div [ngStyle]="{'background': 'red'}"></div>
```

# 管道
1. {{ today | date: 'yyyy-MM-dd' }}

# 事件
1. <button (click)="handleClick()"></button>
2. <button (click)="handleClick($event)"></button>
```html
<!-- ionic必须这样写，指定类型 -->
let dom:any = event.target;
dom.style.color = 'red';
```

# 双向数据绑定
1. app.module.ts中引入`import { FormsModule } from '@angular/forms'`
2. 在imports中声明
3. input使用[(ngModel)]="变量名"

# 服务
1. 定义的方法可以在多个组件使用
2. 使用时只需要在对应的组件引用即可
3. ng g service 服务名称
4. ng g servirce services/storage // 创建到指定目录下面
5. 在app.module.ts中进行引入，然后配置在providers

# angular中的DOM操作
1. ngOnInit() 组件和指令初始化完成
2. ngAfterViewInit() 视图加载完成之后触发，在这里获取dom
3. <div #myBox></div>
4. 组件中引入ViewChild
5. @ViewChild('myBox) myBox:any;
6. this.myBox.nativeElement 获取到DOM

# 获取组件实例
1. <app-header #header></app-header>
2. @ViewChild('header) Header:any;
3. this.header.run() // 父组件获取子组件的方法

# angular中如何执行CSS3动画
1. 拿到dom元素设置其transform

# 组件通讯-父传子
1. <app-header [title]="title" [run]="run" [home]="this"></app-header> // run是父组件的一个方法，title是一个属性
2. 子组件引入Input模块
3. @Input title:any;
4. @Input run:any;
5. @Input home:any // 把父组件的整个实例传递给子组件
6. 通过this.title，this.run()执行调用

# 父组件通过@ViewChild主动获取子组件的数据和方法
1. 父组件：<app-forrter #footer></app-footer>
2. 引入ViewChild
3. @ViewChild('footer') Footer:any;
4. 通过this.Footer调用子组件的方法和属性

# 子组件通过@Output触发父组件的方法
1. 子组件引入`import {Output, EventEmitter} from '@angular/core'`
2. 子组件：`@Output private outer = new EventEmitter()`
3. 子组件：`this.outer.emit('我是来自子组件的数据')`
4. 父组件：`<app-header (outer)="getData($event)"></app-header>` // 通过$event获取子组件传递过来的数据

# angular的生命周期函数
1. [查看文档](https://v6.angular.live/guide/lifecycle-hooks)
2. `https://v6.angular.live/guide/lifecycle-hooks`

# Rxjs异步数据流编程
1. angular引入了rxjs让异步可控、更加简单
2. 解决异步-通过回调函数
3. 解决异步-通过Promise
4. Rxjs
5. `import {Observable} from 'rxjs'`
```js
getRxjsData() {
  return new Observable<any>((observer) => {
    setTimeout(() => {
      var username = '张三--promise';
      observer.next(username);
    }, 2000)
  })
}
```
6. `this.request.getRxjsData().subscribe((value) => {console.log(value)})`
7. 通过unsubscribe撤回操作，异步方法还没有执行时候
```js
var d = this.request.getRxjsData().subscribe((value) => {console.log(value)})
d.unsubscribe()
```
8. rxjs订阅后多次执行，这一点promise做不到
9. angular6以后使用以前的rxjs方法，必须安装rxjs-compat模块才可以使用map、filter方法
10. angular6后官方使用的是rxjs6的新特性，所以官方给出了一个可以暂时延缓不需要去修改rxjs代码的办法：`npm i rxjs-compat`
11. filter用于过滤数据
12. map用于对数据进行处理
13. `import {map, filter} from 'rxjs/operators'`

# Rxjs延迟执行
```js
import {Observable, fromEvent} from 'rxjs'
import {throttleTime} from 'rxjs/operators'
var btn = document.querySelector('button')
fromEvent(btn, 'click').pipe(throttleTime(1000))
.subscribe(() => {console.log('点击按钮，延迟1s触发')})
```

# angular中的数据交互
1. 在app.module.ts中引入HttpClientModule并注入
2. 注入在imports中
3. 在用到的地方引入`import {HttpClient} from '@angular/common/http'`
4. `constructor(public http: HttpClient) {}`
5. `this.http.get(url).subscribe((res) => {})`
6. 对于post请求，需要引入HttpHeaders(设置请求头需要)
7. `const httpOptions = {headers: new HttpHeaders({ 'Content-Type': 'application/json' })}` // angular需要手动设置请求的类型
8. `this.http.post(url, data, httpOptions).subscribe((res) => {})`

# 通过jsonp获取服务器数据（跨域的一种解决方案）
```js
// 1. 在app.module.ts中引入HttpClientModule、HttpClientJsonpModule模块
import {HttpClientModule, HttpClientJsonpModule} from '@angular/common/http'
// 2. 注入到imports中
imports: [
  HttpClientModule,
  HttpClientJsonpModule
]
// 3.在用到的地方引入HttpClient并在构造函数constructor中声明
// 4. 验证服务器api是否支持jsonp，http:// xxx?callback=yyy
this.http.jsonp(url, 'callback'<PS：有些地方是cb>).subscribe(res => {})
```

# 使用Axios
1. npm i axios
2. 引入
3. 新建一个服务封装请求
4. 在app.module.ts中引入该服务，在provider声明
5. 哪里使用就在哪里引用，并且在构造函数中声明

# 路由router
1. 路由就是根据不同的url地址，动态的让根组件挂载其他组件来实现一个单页面的应用
2. app-routing.module.ts 路由配置模块
3. 配置路由(先得引入组件)
```js
const routes: Routes = [
  {
    path: '', // 默认加载
    component: HomeComponent
  },
  {
    path: 'home/:id', // 动态路由
    component: HomeComponent
  },
  {
    path: '**', // 未匹配到路由时跳转到首页
    redirectTo: 'home'
  }
]
```
4. 注入
```js
@NgModule({
  imports: [RouterModule.forRoot(routes)]
})
```
5. <a routerLink="/home"></a> | <a [routerLink]="['/home']"></a>
6. 路由选中，<a routerLink="/home" routerLinkActive="active"></a>
7. get传参，<a routerLink="/home" [queryParams]="{id: item.id}">go detail page</a>
8. 获取get的url参数，引入`import {ActivatedRoute} from '@angular/router'`
9. 在构造函数中声明，`constructor(public route: ActivatedRoute)`
10. `this.route.queryParams.subscribe((data) => {})`
11. 动态路由，<a [routerLink]="['/home', id]"></a>
12. 获取动态路由的url传值，`this.route.params.subscribe((data) => {})`
13. 在业务逻辑中实现页面跳转，首先需要引入`import {Router}from '@angular/router'`，this.router.navigate(['/home', 1])` // 如果不是动态路由，地不用传递第二个参数
14. 路由get传值js跳转，除了引入Router之外，还需要引入NavigationExtras
```js
// 跳转并进行get传值
const params:NavigationExtras = {
  queryParams: {
    id: 1
  }
}
// 如果不指定参数类型为NavigationExtras也是OK的，但是不规范
this.router.navigate(['/home'], params)
```

# 父子路由（嵌套路由）
```js
{
  path: 'home',
  component: HomeComponent,
  children: [
    {
      path: 'setting',
      component: SettingComponent
    },
    {
      path: '**',
      redirectTo: 'home'
    }
  ]
}
```

# angular中的内置模块
1. @angular/core 核心模块
2. @angular/common 通用模块
3. @angular/forms 表单模块
4. @angular/http 网络模块
5. ...

# angular中的自定义模块
1. 组件很多，导致加载速度变慢
2. 按需加载各个组件模块
3. 默认加载根模块，其余的放在自定义模块
4. 创建模块：`ng g module 模块名称`
5. 用angular编写应用时，一般来说每个页面可以看作一个组件，如果某一个页面比较复杂，可以把这个页面看作一个模块来处理，eg：`ng g module module/user`，在这个模块中新建一个根组件，eg：`ng g component module/user`，编写该页面的入口，在当前模块下面新建其他组件作为其它子页面，eg：`ng g component module/user/components/plist`，各个组件存放在`user.module.ts`文件中，不会默认注入到根模块，通过`exports`暴露组件，让其它模块可以使用，在根模块引入自定义模块，注入到`imports`中

# 模块懒加载
1. 在app.module.ts中配置路由
```js
{
  path: 'user',
  loadChildren: './module/user/user.module#UserModule'
}
```
2. 在自定义模块的路由文件中配置
```js
{
  path: '',
  component: UserComponent,
  children: [
    {
      path: 'profile',
      component: ProfileComponent
    }
  ]
},
// 在user.component.html
// <router-outlet></router-outlet>
{
  path: 'cart',
  component: CartComponent
}
// 这样就会出现在app.component.html
```
