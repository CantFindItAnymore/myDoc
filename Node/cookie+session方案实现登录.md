### 1. 需求分析

核心：登录校验，登录信息存储。

### 2. 学在前面

#### A. `cookie`

- **介绍：**

  - 存储在浏览器的一串字符串（最大`5kb`）。

  - 跨域不共享。

  - 格式

    ```javascript
    k1=v1;k2=v2;k3=v3;
    
    // 可以存储结构化数据
    ```

  - 每次发送`ajax`请求，都会将请求域的`cookie`一起发送给`server`。
  - `server`端可以修改`cookie`并返回给浏览器。
  - 浏览器也可以用`js`修改`cookie`（有限制）。

- **浏览器中查看`cookie`（3种方式）**

  - `F12 → Network → Request Header → Cookie`

  - `F12 → Application → Cookies`

    ```javascript
    Name // cookie的键
    Value // cookie的值
    Domain // cookie生效的域名
    Path // cookie生效的路径 /根路径：代表该域名下所有路径都生效
    Expires/Max-Age // cookie的有效期
    Size // cookie的大小
    HttpOnly // cookie的
    Secure // cookie的
    SameSite // cookie的
    
    ```

  - `F12 → Console → document.cookie`

- **客户端`js`修改`cookie`（一种方式，有限制）**

  `F12 → Console → document.cookie='rj=yy'`

  *注意这种修改方式是追加  `cookie`  而不是修改  `cookie`* 

- **server端用`nodejs`操作`cookie`（实现登录验证）**

  - 查看cookie

    ```javascript
    /*
    	app.js
    */
    
    // 获取cookie
      req.cookie = {}
      const cookieStr = req.headers.cookie || ''
      cookieStr.split(';').map(item => {
        if (!item) {
          return false
        }
        const arr = item.split('=')
        const key = arr[0]
        const value = arr[1]
        req.cookie[key] = value
      })
      console.log('req.cookie: ', req.cookie)
    ```

    *注意该方案中，若`cookie`中有`username`字段，则我们认为用户已登录。所以，我们需要能在server端修改cookie。*

  - 修改`cookie`

  - 实现登录验证

------

#### B. `session`

#### C. `redis`

- 介绍：
  - 内存数据库（对比硬盘数据库如：`mysql`），这里用来存放`session。`

#### D. `Nginx`

- 反向代理

### 3. 实现登录功能

