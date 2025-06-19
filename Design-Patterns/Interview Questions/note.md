在计算机面试中，**设计模式**是一个经常被问及的话题，尤其是在中高级工程师、架构师岗位的面试中。面试官通过这些问题考察你对**软件设计原则、可扩展性、可维护性**的理解。以下是常见的设计模式面试问题，按主题分类，并附上每类代表性问题：

---

## 一、设计模式基础类问题

1. **什么是设计模式？为什么要使用设计模式？**
2. **设计模式有哪些类型？请举例说明。**

   - 创建型、结构型、行为型

3. **设计模式和架构模式有什么区别？**
4. **你在项目中用过哪些设计模式？请详细说一个例子。**
5. **设计模式有什么优缺点？**

---

## 二、创建型模式相关问题

1. **什么是单例模式？如何实现线程安全的单例？**

   - 饿汉式、懒汉式、双重检查锁、静态内部类等

2. **工厂模式和抽象工厂模式有什么区别？什么时候使用？**
3. **建造者模式适用于什么场景？你在哪个项目中使用过？**
4. **原型模式如何实现深拷贝？和浅拷贝的区别？**

---

## 三、结构型模式相关问题

1. **适配器模式和装饰器模式的区别是什么？能举例说明吗？**
2. **代理模式有哪些种类？静态代理和动态代理有何不同？**

   - Java 中的 `JDK Proxy` 和 `CGLIB` 的区别

3. **组合模式适用于什么样的场景？能举个实际例子吗？**
4. **桥接模式是如何解耦多个维度变化的？**

---

## 四、行为型模式相关问题

1. **观察者模式的应用场景有哪些？在项目中是如何使用的？**

   - 比如事件通知、发布-订阅等

2. **策略模式和状态模式的区别是什么？**
3. **责任链模式适用于哪些业务场景？如何设计？**
4. **模板方法模式和策略模式的区别？如何选择？**
5. **命令模式的结构是什么？可以举一个实际例子吗？**

---

## 五、综合类与对比类问题

1. **策略模式 vs 简单工厂模式？有什么区别？**
2. **观察者模式 vs 事件驱动模型？**
3. **装饰器模式 vs 继承？**
4. **代理模式 vs 装饰器模式 vs 适配器模式 有何异同？**

---

## 六、项目实战类问题

1. **请结合你过往的项目，说一个使用设计模式解决实际问题的例子。**
2. **你遇到过设计过度的问题吗？设计模式用多了怎么办？**
3. **你是怎么让你的代码易于扩展的？有没有用设计模式？**
4. **有没有遇到过设计上的冲突？是怎么解决的？**

---

## 一、设计模式基础类问题

### 1. 什么是设计模式？为什么要使用设计模式？

**答：**
设计模式是前人总结出来的、针对特定软件设计问题的通用解决方案。它们并不是可以直接运行的代码，而是一种设计思想或代码结构的模板。

**使用原因：**

- 提高代码的可读性和可维护性
- 促进代码的重用和解耦
- 减少 bug，提高开发效率
- 让团队成员在架构层面有共同语言

---

### 2. 设计模式有哪些类型？请举例说明。

**答：**
设计模式主要分为三大类：

| 类型   | 代表模式                   | 说明                         |
| ------ | -------------------------- | ---------------------------- |
| 创建型 | 单例、工厂、建造者、原型   | 关注对象如何创建             |
| 结构型 | 适配器、装饰器、代理、桥接 | 关注类或对象的组合结构       |
| 行为型 | 策略、观察者、状态、命令   | 关注对象之间的通信与职责划分 |

---

### 3. 设计模式和架构模式有什么区别？

**设计模式：**

- 代码级别，解决类、对象的创建与协作问题。
- 如：单例、工厂、观察者等。

**架构模式：**

- 系统级别，指导系统整体架构。
- 如：MVC、MVVM、微服务、CQRS 等。

---

### 4. 你在项目中用过哪些设计模式？请详细说一个例子。

**举例：使用观察者模式实现一个事件总线（JavaScript）**

```js
class EventEmitter {
  constructor() {
    this.events = {};
  }
  on(event, listener) {
    (this.events[event] ||= []).push(listener);
  }
  emit(event, ...args) {
    (this.events[event] || []).forEach((fn) => fn(...args));
  }
  off(event, listener) {
    this.events[event] = (this.events[event] || []).filter((fn) => fn !== listener);
  }
}
```

---

### 5. 设计模式有什么优缺点？

**优点：**

- 提高可维护性、可扩展性
- 降低耦合
- 提升开发效率

**缺点：**

- 初学者理解较困难
- 滥用可能造成设计过度、增加复杂度
- 会降低代码的直观性（尤其是滥用抽象时）

---

## 二、创建型模式相关问题

---

### 1. **什么是单例模式？如何实现线程安全的单例？**

#### ✅ 答：

**单例模式（Singleton Pattern）**：保证一个类只有一个实例，并提供一个全局访问点。

**用途：**

- 日志记录器
- 配置管理
- 数据库连接池

#### ✅ JavaScript 实现（天然单线程）：

```js
class Singleton {
  constructor() {
    if (Singleton.instance) return Singleton.instance;
    this.value = Math.random(); // 模拟资源
    Singleton.instance = this;
  }
}

const a = new Singleton();
const b = new Singleton();
console.log(a === b); // true
```

#### ✅ Go 实现（支持并发，需线程安全）：

```go
package singleton

import (
	"fmt"
	"sync"
)

type singleton struct {
	value int
}

var instance *singleton
var once sync.Once

func GetInstance() *singleton {
	once.Do(func() {
		fmt.Println("Creating instance...")
		instance = &singleton{value: 42}
	})
	return instance
}
```

调用：

```go
s1 := singleton.GetInstance()
s2 := singleton.GetInstance()
fmt.Println(s1 == s2) // true
```

---

### 2. **工厂模式和抽象工厂模式有什么区别？什么时候使用？**

#### ✅ 答：

| 模式         | 描述                                   | 举例                                 |
| ------------ | -------------------------------------- | ------------------------------------ |
| 工厂方法模式 | 父类定义接口，子类决定具体返回哪个产品 | `ShapeFactory` -> 圆形、方形         |
| 抽象工厂模式 | 提供多个产品族的工厂，不指定具体类     | `GUIFactory` -> `Button`, `Checkbox` |

#### ✅ JavaScript 工厂方法示例：

```js
class Circle {
  draw() {
    console.log("Drawing circle");
  }
}
class Square {
  draw() {
    console.log("Drawing square");
  }
}
function ShapeFactory(type) {
  if (type === "circle") return new Circle();
  if (type === "square") return new Square();
}
ShapeFactory("circle").draw(); // Drawing circle
```

#### ✅ Go 抽象工厂示例：

```go
type Button interface {
	Paint()
}
type WinButton struct{}
func (b *WinButton) Paint() { fmt.Println("Windows Button") }

type MacButton struct{}
func (b *MacButton) Paint() { fmt.Println("Mac Button") }

type GUIFactory interface {
	CreateButton() Button
}

type WinFactory struct{}
func (f *WinFactory) CreateButton() Button { return &WinButton{} }

type MacFactory struct{}
func (f *MacFactory) CreateButton() Button { return &MacButton{} }
```

调用：

```go
func render(factory GUIFactory) {
	btn := factory.CreateButton()
	btn.Paint()
}
render(&WinFactory{}) // Windows Button
```

---

### 3. **建造者模式适用于什么场景？你在哪个项目中使用过？**

#### ✅ 答：

**建造者模式（Builder Pattern）**：用于构建一个复杂对象的各个部分，使构造过程和表示解耦。

**应用场景：**

- 构造步骤固定但组合复杂（如配置对象、表单）
- 想要支持链式调用

#### ✅ JavaScript 示例（链式构建）：

```js
class CarBuilder {
  constructor() {
    this.car = {};
  }
  setEngine(engine) {
    this.car.engine = engine;
    return this;
  }
  setColor(color) {
    this.car.color = color;
    return this;
  }
  build() {
    return this.car;
  }
}

const car = new CarBuilder().setEngine("V8").setColor("red").build();
console.log(car); // { engine: 'V8', color: 'red' }
```

#### ✅ Go 示例：

```go
type Car struct {
	Engine string
	Color  string
}

type CarBuilder struct {
	car *Car
}

func NewCarBuilder() *CarBuilder {
	return &CarBuilder{car: &Car{}}
}
func (b *CarBuilder) SetEngine(engine string) *CarBuilder {
	b.car.Engine = engine
	return b
}
func (b *CarBuilder) SetColor(color string) *CarBuilder {
	b.car.Color = color
	return b
}
func (b *CarBuilder) Build() *Car {
	return b.car
}
```

调用：

```go
car := NewCarBuilder().SetEngine("V6").SetColor("Blue").Build()
fmt.Println(car)
```

---

### 4. **原型模式如何实现深拷贝？和浅拷贝的区别？**

#### ✅ 答：

**原型模式（Prototype Pattern）**：通过复制现有对象来创建新对象，避免繁琐的初始化过程。

- **浅拷贝**：复制对象的字段引用（可能共享内存）
- **深拷贝**：复制所有字段及嵌套对象，互不影响

#### ✅ JavaScript 示例：

```js
const obj = {
  name: "Nico",
  data: { age: 18 },
};

const shallow = { ...obj };
shallow.data.age = 20;
console.log(obj.data.age); // 20

const deep = JSON.parse(JSON.stringify(obj));
deep.data.age = 30;
console.log(obj.data.age); // 20
```

#### ✅ Go 示例：

Go 没有内建 clone，你可以手动实现：

```go
type Profile struct {
	Name string
	Data map[string]int
}

func (p *Profile) Clone() *Profile {
	dataCopy := make(map[string]int)
	for k, v := range p.Data {
		dataCopy[k] = v
	}
	return &Profile{Name: p.Name, Data: dataCopy}
}
```

调用：

```go
p1 := &Profile{Name: "Nico", Data: map[string]int{"age": 18}}
p2 := p1.Clone()
p2.Data["age"] = 30
fmt.Println(p1.Data["age"]) // 18
```

---

继续为你讲解设计模式面试题，进入：

---

## 三、结构型模式相关问题

结构型模式关注对象之间的组合关系，常用于**封装、扩展和适配现有结构**。

---

### 1. **适配器模式和装饰器模式的区别是什么？能举例说明吗？**

#### ✅ 答：

| 特点             | 适配器模式                     | 装饰器模式                           |
| ---------------- | ------------------------------ | ------------------------------------ |
| 目的             | 让原本接口不兼容的类能一起工作 | 在不修改对象结构的情况下动态扩展功能 |
| 使用时机         | 接口不兼容但需要协作           | 想要给对象添加额外功能               |
| 是否改变原始结构 | 是（转换接口）                 | 否（增强功能）                       |

---

#### ✅ JavaScript 适配器模式示例：

```js
class OldPrinter {
  printText() {
    console.log("Printing from old printer...");
  }
}

class NewPrinter {
  print() {
    console.log("Printing from new printer...");
  }
}

// 适配器：让 NewPrinter 兼容 OldPrinter 接口
class PrinterAdapter {
  constructor(newPrinter) {
    this.newPrinter = newPrinter;
  }
  printText() {
    this.newPrinter.print();
  }
}

// 使用
const printer = new PrinterAdapter(new NewPrinter());
printer.printText(); // Printing from new printer...
```

---

#### ✅ JavaScript 装饰器模式示例：

```js
class Coffee {
  cost() {
    return 5;
  }
}

class MilkDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }
  cost() {
    return this.coffee.cost() + 2;
  }
}

let coffee = new Coffee();
coffee = new MilkDecorator(coffee);
console.log(coffee.cost()); // 7
```

---

#### ✅ Go 适配器示例：

```go
type OldPlayer interface {
	PlayOld()
}

type NewPlayer struct{}

func (p *NewPlayer) PlayNew() {
	fmt.Println("Playing with new player")
}

type Adapter struct {
	newPlayer *NewPlayer
}

func (a *Adapter) PlayOld() {
	a.newPlayer.PlayNew()
}
```

---

### 2. **代理模式有哪些种类？静态代理和动态代理有何不同？**

#### ✅ 答：

**代理模式（Proxy Pattern）**：为某个对象提供一个替代访问控制的接口。

**应用场景：**

- 控制访问权限
- 延迟加载
- 日志、缓存、安全验证

#### ✅ 分类：

- **静态代理**：写死代理逻辑（Go 常用）
- **动态代理**：运行时生成（JS 可通过 Proxy 动态实现）

---

#### ✅ JavaScript 动态代理示例：

```js
const user = {
  name: "Nico",
  role: "guest",
};

const securedUser = new Proxy(user, {
  get(target, prop) {
    if (prop === "secret") {
      throw new Error("Access denied");
    }
    return target[prop];
  },
});

console.log(securedUser.name); // Nico
console.log(securedUser.secret); // Error: Access denied
```

---

#### ✅ Go 静态代理示例：

```go
type Downloader interface {
	Download()
}

type RealDownloader struct{}
func (r *RealDownloader) Download() {
	fmt.Println("Downloading file...")
}

type ProxyDownloader struct {
	real *RealDownloader
}
func (p *ProxyDownloader) Download() {
	fmt.Println("Checking permissions...")
	p.real.Download()
}
```

---

### 3. **组合模式适用于什么样的场景？能举个实际例子吗？**

#### ✅ 答：

**组合模式（Composite Pattern）**：将对象组合成树状结构，使用户对单个对象和组合对象使用一致的方式。

**适用场景：**

- 文件系统目录/文件
- UI 组件树
- JSON 树结构解析

---

#### ✅ JavaScript 示例（文件系统结构）：

```js
class File {
  constructor(name) {
    this.name = name;
  }
  display() {
    console.log(this.name);
  }
}

class Folder {
  constructor(name) {
    this.name = name;
    this.children = [];
  }
  add(child) {
    this.children.push(child);
  }
  display() {
    console.log(this.name);
    this.children.forEach((child) => child.display());
  }
}

const root = new Folder("root");
const file1 = new File("file1.txt");
const subFolder = new Folder("sub");
const file2 = new File("file2.txt");

subFolder.add(file2);
root.add(file1);
root.add(subFolder);
root.display();
```

---

#### ✅ Go 示例（简化版组件树）：

```go
type Component interface {
	Display()
}

type File struct {
	Name string
}

func (f *File) Display() {
	fmt.Println(f.Name)
}

type Folder struct {
	Name     string
	Children []Component
}

func (f *Folder) Display() {
	fmt.Println(f.Name)
	for _, child := range f.Children {
		child.Display()
	}
}
```

---

### 4. **桥接模式是如何解耦多个维度变化的？**

#### ✅ 答：

**桥接模式（Bridge Pattern）**：将抽象和实现解耦，使两者可以独立扩展。

**典型场景：** 你有两个维度都可能变化：比如**平台**和**UI 控件**

---

#### ✅ JavaScript 示例：

```js
class DrawAPI1 {
  drawCircle(x, y, r) {
    console.log(`API1 Drawing circle at (${x}, ${y}) with radius ${r}`);
  }
}

class DrawAPI2 {
  drawCircle(x, y, r) {
    console.log(`API2 Drawing circle at (${x}, ${y}) with radius ${r}`);
  }
}

class Circle {
  constructor(x, y, r, drawAPI) {
    this.x = x;
    this.y = y;
    this.r = r;
    this.drawAPI = drawAPI;
  }

  draw() {
    this.drawAPI.drawCircle(this.x, this.y, this.r);
  }
}

const c1 = new Circle(0, 0, 10, new DrawAPI1());
const c2 = new Circle(0, 0, 10, new DrawAPI2());
c1.draw();
c2.draw();
```

---

#### ✅ Go 示例：

```go
type DrawAPI interface {
	DrawCircle(x, y, r int)
}

type RedDraw struct{}
func (r *RedDraw) DrawCircle(x, y, r1 int) {
	fmt.Printf("Drawing RED circle at (%d, %d) with radius %d\n", x, y, r1)
}

type Circle struct {
	x, y, radius int
	drawer       DrawAPI
}

func (c *Circle) Draw() {
	c.drawer.DrawCircle(c.x, c.y, c.radius)
}
```

调用：

```go
circle := Circle{10, 10, 5, &RedDraw{}}
circle.Draw()
```

---

## 四、行为型设计模式相关问题

行为型模式关注**对象间的通信与职责划分**。

---

### 1. **观察者模式的应用场景有哪些？在项目中是如何使用的？**

#### ✅ 答：

**观察者模式（Observer Pattern）**：对象之间一对多依赖关系，一旦被观察者状态变化，所有观察者自动收到通知。

**应用场景：**

- 事件总线 / 发布-订阅系统
- GUI 组件（按钮点击通知监听器）
- Vue.js 响应式系统
- Go 中用于通知更新（例如监听配置变动）

---

#### ✅ JavaScript 示例（自定义事件总线）：

```js
class EventEmitter {
  constructor() {
    this.events = {};
  }
  on(event, listener) {
    (this.events[event] ||= []).push(listener);
  }
  emit(event, ...args) {
    (this.events[event] || []).forEach((fn) => fn(...args));
  }
  off(event, listener) {
    this.events[event] = (this.events[event] || []).filter((fn) => fn !== listener);
  }
}

// 使用
const emitter = new EventEmitter();
function greet(name) {
  console.log(`Hello, ${name}`);
}
emitter.on("hello", greet);
emitter.emit("hello", "Nico"); // Hello, Nico
```

---

#### ✅ Go 示例（简化观察者）：

```go
type Observer interface {
	Update(data string)
}

type Subject struct {
	observers []Observer
}

func (s *Subject) Add(o Observer) {
	s.observers = append(s.observers, o)
}
func (s *Subject) Notify(data string) {
	for _, o := range s.observers {
		o.Update(data)
	}
}

type User struct {
	name string
}
func (u *User) Update(data string) {
	fmt.Printf("%s received: %s\n", u.name, data)
}
```

调用：

```go
subject := &Subject{}
user1 := &User{name: "Nico"}
user2 := &User{name: "Lin"}

subject.Add(user1)
subject.Add(user2)

subject.Notify("New message")
```

---

### 2. **策略模式和状态模式的区别是什么？**

#### ✅ 答：

| 模式   | 策略模式（Strategy）       | 状态模式（State）                    |
| ------ | -------------------------- | ------------------------------------ |
| 意图   | 根据上下文选择不同算法     | 根据状态切换行为（封装状态及行为）   |
| 场景   | 计算方式变化、支付方式变化 | 状态机、工作流，如订单状态、播放状态 |
| 控制流 | 通过传入策略切换           | 通过状态内部转移控制                 |

---

#### ✅ JavaScript 策略模式：

```js
const strategies = {
  add: (a, b) => a + b,
  mul: (a, b) => a * b,
};

function execute(strategy, a, b) {
  return strategies[strategy](a, b);
}

console.log(execute("add", 1, 2)); // 3
```

---

#### ✅ Go 策略模式：

```go
type Strategy func(a, b int) int

func Execute(a, b int, strategy Strategy) int {
	return strategy(a, b)
}

func Add(a, b int) int {
	return a + b
}
```

调用：

```go
res := Execute(3, 4, Add)
fmt.Println(res) // 7
```

---

#### ✅ JavaScript 状态模式：

```js
class State {
  constructor(name) {
    this.name = name;
  }
  handle(context) {
    console.log(`State: ${this.name}`);
    context.setState(new State(this.name === "A" ? "B" : "A"));
  }
}

class Context {
  constructor(state) {
    this.state = state;
  }
  setState(state) {
    this.state = state;
  }
  request() {
    this.state.handle(this);
  }
}

const context = new Context(new State("A"));
context.request(); // A -> B
context.request(); // B -> A
```

---

### 3. **责任链模式适用于哪些业务场景？如何设计？**

#### ✅ 答：

**责任链模式（Chain of Responsibility）**：多个对象按顺序处理请求，每个对象决定是否处理或传递。

**应用场景：**

- 请求过滤（如中间件、日志、权限、参数验证）
- Go 中的 HTTP 中间件链（`net/http`）
- JS Express/Koa 中间件机制

---

#### ✅ JavaScript 示例（中间件）：

```js
class Middleware {
  constructor() {
    this.stack = [];
  }
  use(fn) {
    this.stack.push(fn);
  }
  execute(context) {
    const dispatch = (i) => {
      if (i < this.stack.length) {
        this.stack[i](context, () => dispatch(i + 1));
      }
    };
    dispatch(0);
  }
}

const app = new Middleware();
app.use((ctx, next) => {
  console.log("auth check");
  next();
});
app.use((ctx, next) => {
  console.log("logging");
  next();
});
app.execute({});
```

---

#### ✅ Go 示例（中间件链）：

```go
type HandlerFunc func(next HandlerFunc) HandlerFunc

func Auth(next HandlerFunc) HandlerFunc {
	return func(n HandlerFunc) HandlerFunc {
		return func() {
			fmt.Println("Auth check")
			next(nil)()
		}
	}
}

func Final(_ HandlerFunc) HandlerFunc {
	return func() {
		fmt.Println("Final handler")
	}
}
```

简化说明链式执行，中间件就是责任链。

---

### 4. **模板方法模式和策略模式的区别？如何选择？**

#### ✅ 答：

| 模式     | 模板方法模式                               | 策略模式                               |
| -------- | ------------------------------------------ | -------------------------------------- |
| 定义方式 | 基类定义算法骨架，子类实现步骤             | 封装多个算法实现，传入使用             |
| 适合场景 | 固定流程，部分步骤由子类决定（如登录流程） | 多个行为可选，如“排序方式”、“支付方式” |

---

#### ✅ JavaScript 模板方法模式：

```js
class AbstractLogger {
  log() {
    this.before();
    this.write();
    this.after();
  }
  before() {
    console.log("Start");
  }
  after() {
    console.log("End");
  }
}

class FileLogger extends AbstractLogger {
  write() {
    console.log("Writing to file");
  }
}

new FileLogger().log();
```

---

### 5. **命令模式的结构是什么？可以举一个实际例子吗？**

#### ✅ 答：

**命令模式（Command Pattern）**：将请求封装成命令对象，支持撤销、记录日志、排队等功能。

**结构：**

- Command 接口：定义执行操作
- ConcreteCommand：封装请求
- Receiver：具体执行者
- Invoker：请求者

---

#### ✅ JavaScript 示例：

```js
class Light {
  on() {
    console.log("Light ON");
  }
  off() {
    console.log("Light OFF");
  }
}

class LightOnCommand {
  constructor(light) {
    this.light = light;
  }
  execute() {
    this.light.on();
  }
}

class Remote {
  setCommand(cmd) {
    this.cmd = cmd;
  }
  press() {
    this.cmd.execute();
  }
}

const light = new Light();
const onCommand = new LightOnCommand(light);
const remote = new Remote();
remote.setCommand(onCommand);
remote.press(); // Light ON
```

---

#### ✅ Go 简化命令模式：

```go
type Command interface {
	Execute()
}

type Light struct{}

func (l *Light) On() {
	fmt.Println("Light ON")
}

type LightOnCommand struct {
	light *Light
}

func (c *LightOnCommand) Execute() {
	c.light.On()
}
```

调用：

```go
light := &Light{}
cmd := &LightOnCommand{light}
cmd.Execute()
```

---
