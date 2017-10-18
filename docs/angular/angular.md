# AngularJS实战
1. MVC
2. 模块化，依赖注入
3. 双向数据绑定
4. 指令系统

angularJS directive特性---指令系统

> anglualar-cli 工程结构（注意查阅）
> https://angular.cn/guide/quickstart

# Angular
## 导航路径
![Alt text](./导航路径.png)

> 注意语法,空格等严格的控制

ts文件就是一个个模块
所以整个项目就是由一个个ts拼凑而成的

1. ts文件方法
```javascript
步骤一：
import { 类名（驼峰式） } from '路径+文件名（除去后缀ts）';

步骤二：导入装饰器（可选）
写法一： @Component({
	selector: '',
	template: `
	
	`,
})
写法二： @Injectable()---服务中使用

步骤三：书写类
export class HeroService {
	
	步骤四：书写桩方法
	方法名(): 返回参数 { }
}

```

## 服务
mock-heroes.ts
hero.service.ts---@Injectable


构造函数取代new 实例
你可以用两行代码代替用new时的一行：
- 添加一个构造函数，并定义一个私有属性。
- 添加组件的providers元数据。

> providers数组告诉 Angular，当它创建新的AppComponent组件时，也要创建一个HeroService的新实例。 AppComponent会使用那个服务来获取英雄列表，在它组件树中的每一个子组件也同样如此。

我们把回调函数作为参数传给承诺对象的then方法：
```
getHeroes(): void {
  this.heroService.getHeroes().then(heroes => this.heroes = heroes);
}
```

> 回调中所用的 **ES2015 箭头函数**  比等价的函数表达式更加简洁，能优雅的处理 this 指针。

## ngOnInit 生命周期钩子
毫无疑问，AppComponent应该获取英雄数据并显示它。

你可能想在构造函数中调用getHeroes()方法，但构造函数不应该包含复杂的逻辑，特别是那些需要从服务器获取数据的逻辑更是如此。构造函数是为了简单的初始化工作而设计的，例如把构造函数的参数赋值给属性。

只要我们实现了 Angular 的 ngOnInit 生命周期钩子，**Angular 就会主动调用这个钩子。 Angular提供了一些接口，用来介入组件生命周期的几个关键时间点：刚创建时、每次变化时，以及最终被销毁时。**

每个接口都有唯一的一个方法。只要组件实现了这个方法，Angular 就会在合适的时机调用它。

## 路由与导航
下面是我们的计划：

- 把AppComponent变成应用程序的“壳”，它只处理导航
- 把现在由AppComponent关注的英雄们移到一个独立的HeroesComponent中
- 添加路由
- 创建一个新的DashboardComponent组件
- 把仪表盘加入导航结构中

> 路由是导航的另一个名字。路由器就是从一个视图导航到另一个视图的机制。

创建 AppComponent

新的AppComponent将成为应用的“壳”。 它将在顶部放一些导航链接，并且把我们要导航到的页面放在下面的显示区中。

执行下列步骤：

- 添加支持性的import语句。
- 创建一个名叫src/app/app.component.ts的新文件。
- 定义一个导出的 AppComponent类。
- 在类的上方添加@Component元数据装饰器，装饰器带有my-app选择器。
- 将下面的项目从HeroesComponent移到AppComponent：
- - title类属性
- - @Component模板中的< h1>标签，它包含了对title属性的绑定。
- 在模板的标题下面添加< my-heroes>标签，以便我们仍能看到英雄列表。
- 添加HeroesComponent组件到根模块的declarations数组中，以便 Angular 能认识< my-heroes>标签。
- 添加HeroService到AppModule的providers数组中，因为我们的每一个视图都需要它。
- 从HerosComponent的providers数组中移除HeroService，因为它被提到模块了。
- 为AppComponent添加一些import语句。

> 这里使用了forRoot()方法，因为我们是在应用根部提供配置好的路由器。 forRoot()方法提供了路由需要的路由服务提供商和指令，并基于当前浏览器 URL 初始化导航。

路由需要在app.module.ts文件的@NgModule 中实现路由，然后在利用RouterModule.forRoot()方法

### 配置带参数的路由
