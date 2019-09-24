*注意编程思维一定要从客户端转向 server端*

### 一. ES基础

#### 1. 类型判断

- typeof

  ```javascript
  console.log(typeof 1);  // number
  console.log(typeof '1');  //string
  console.log(typeof true);  //boolean
  console.log(typeof null); //object-------------------------
  console.log(typeof undefined); //undefined
  console.log(typeof obj); //object
  console.log(typeof foo);  //function
  console.log(typeof arr);   //object----------------------------
  ```

- instanceof

  instanceof操作符主要用来检查构造函数的原型是否在对象的原型链上。

  适合用于判断自定义的类实例对象。

  ```javascript
  [] instanceof Array // true
  ```

- constructor

- toString

  ```javascript
  function foo(){};
  
  Object.prototype.toString.call(1);  '[object Number]'
  Object.prototype.toString.call('1'); '[object String]'
  Object.prototype.toString.call(NaN); '[object Number]'
  Object.prototype.toString.call(foo);  '[object Function]'
  Object.prototype.toString.call([1,2,3]); '[object Array]'
  Object.prototype.toString.call(undefined); '[object Undefined]'
  Object.prototype.toString.call(null); '[object Null]'
  Object.prototype.toString.call(true); '[object Boolean]'
  ....
  ```

#### 2. 作用域

#### 3. 引用传递 / 值传递

#### 4. 内存释放

#### 5. ES 6新特性

### 二. 模块

#### 1. 模块机制

#### 2. 热更新

#### 3. 上下文

#### 4.包管理

### 三. 事件/异步

#### 1. Promise

#### 2. Event

#### 3. Timer

#### 4. 阻塞 / 异步

#### 5. 并行 / 并发

### 四. 进程

#### 1. Process

#### 2. Child Process

#### 3. Cluster

#### 4. 进程间通信

#### 5. 进程守护

### 五. IO

#### 1. Buffer

#### 2. String Decoder

#### 3. Steam

#### 4. Console

#### 5. File System

#### 6.  Read Line

#### 7. REPL

### 六. Network

#### 1. Net

#### 2. UDP

#### 3. HTTP

#### 4. DNS

#### 5. ZLIB

#### 6. RPC



