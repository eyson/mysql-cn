# 安装

在安装之前，请确保你的 Node.js 版本 > `0.6`，使用以下命令完成安装。

```shell
npm install mysql
```

有关 `0.9.x` 版本的信息，请访问 [v0.9分支](https://github.com/mysqljs/mysql/tree/v0.9)。

有时我可能会要求你从 `Github` 上安装最新版本以测试问题是否修复。在这种情况下，你需要执行以下安装命令：

```shell
npm install mysqljs/mysql
```



# 介绍

这是一个基于 `Node.js` 的 `MySQL` 驱动程序，它是用 `JavaScript` 编写的，不需要编译。

我们来看个简单的使用示例：

```js
// 加载模块
let mysql = require('mysql');

// 数据库连接配置
let conn = mysql.createConnection({
    host: 'localhost',
    user: 'username',
    password: 'yourpassword',
    database: 'mydb'
});

// 连接数据
conn.connect();

// 查询用户表数据
conn.query('SELECT * FROM user;', function(err, res, rows){
    if(err){
        // 异步处理
        throw err;
    }else{
        // 查询成功，处理数据
        console.log(rows);
    }
});

// 关闭数据库连接
// 数据不关闭会造成连接池过大，导致事故哦
conn.end();
```

在此示例中，你可以了解以下内容：

- 每次数据查询都需要按顺序排队执行
- 每次数据查询之后必须关闭连接池，以确保其它排队成员可以正常查询

# 贡献者

感谢为此模块提供代码的人员，请参阅[Github贡献者页面](https://github.com/mysqljs/mysql/graphs/contributors)。

另外，我要感觉以下人员：

- Andrey Hristov (Oracle) - 帮我解决协议问题
- Ulf Wendel (Oracle) - 帮我解决协议问题

# 社区

如果您想讨论此模块，或者询问有关问题，请使用以下方法之一。

- 邮件列表（<https://groups.google.com/forum/#!forum/node-mysql>）
- IRC 频道 #node.js (on freenode.net, I pay attention to any message including the term `mysql`)

# 建立连接

我们建议使用的连接方法

```js
// 加载模块
let mysql = require('mysql');

// 数据库连接配置
let conn = mysql.createConnection({
    host: 'localhost',
    user: 'username',
    password: 'yourpassword',
    database: 'mydb'
});

// 连接数据
conn.connect();

// 查询用户表数据
conn.query('SELECT * FROM user;', function(err, res, rows){
    if(err){
        // 异步处理
        throw err;
    }else{
        // 查询成功，处理数据
        console.log(rows);
    }
});

// 关闭数据库连接
// 数据不关闭会造成连接池过大，导致事故哦
conn.end();
```

# 连接配置

建立连接时，你可以设置以下选项。

| 选项               | 说明                                                         | 默认值          |
| ------------------ | ------------------------------------------------------------ | --------------- |
| host               | 要连接的数据库主机名                                         | localhost       |
| port               | 主机端口号                                                   | 3306            |
| localAddress       | 用于 TCP 连接的源 IP 地圵                                    | 可选            |
| socketPath         | 要连接到的unix域套接字的路径。在使用时`host` 和`port`被忽略  | 可选            |
| user               | 要进行身份验证的 MySQL 用户                                  | -               |
| password           | 该 MySQL 用户的密码                                          | -               |
| database           | 用于此连接的数据库的名称                                     | 可选            |
| charset            | 字符集                                                       | utf8_general_ci |
| timezone           | MySQL 服务器上配置的时区                                     | local           |
| connectTimeout     | 连接超时时间，毫秒                                           | 10000           |
| stringifyObjects   | Stringify 对象而不是转换为值，见问题[＃501](https://github.com/mysqljs/mysql/issues/501) | false           |
| insecureAuth       | 允许连接到要求旧（不安全）身份验证方法的 MySQL 实例          | false           |
| typeCast           | 确定是否应将列值转换为本机 JavaScript 类型                   | true            |
| queryFormat        | 自定义查询格式功能，见[自定义格式](https://www.npmjs.com/package/mysql#custom-format) | -               |
| supportBigNumbers  | 在处理数据库中的大数字（BIGINT和DECIMAL列）时，应启用此选项  | false           |
| bigNumberStrings   | 启用两者`supportBigNumbers`并`bigNumberStrings`强制将大数字（BIGINT和DECIMAL列）始终作为 JavaScript String 对象返回 | false           |
| dateStrings        | 强制日期类型（TIMESTAMP，DATETIME，DATE）作为字符串返回，而不是膨胀为 JavaScript Date 对象。可以是 `true`/ `false` 或要保存为字符串的类型名称数组 | false           |
| debug              | 调试模式                                                     | false           |
| trace              | 生成堆栈跟踪 `Error` 以包括库入口的调用站点（“长堆栈跟踪”）  | true            |
| multipleStatements | 每个查询允许多个 MySQL 语句                                  | false           |
| flags              | 使用非默认连接标志的连接标志列表                             | -               |
| ssl                | 带有 `ssl` 参数的对象或包含 `ssl` 配置文件名称的字符串，请参阅 [SSL选项](https://www.npmjs.com/package/mysql#ssl-options) | -               |

#### SSL 选项

将 `ssl` 在连接选项选项需要一个字符串或对象。给定字符串时，它使用包含的预定义`ssl` 配置文件之一。包括以下配置文件：

- `"Amazon RDS"`：此配置文件用于连接到 Amazon RDS 服务器，并包含来自<https://rds.amazonaws.com/doc/rds-ssl-ca-cert.pem> 和 [https://s3.amazonaws.com/rds-](https://s3.amazonaws.com/rds-downloads/rds-combined-ca-bundle.pem) 的证书。[下载/ RDS-组合-CA-bundle.pem](https://s3.amazonaws.com/rds-downloads/rds-combined-ca-bundle.pem)

连接到其他服务器时，您需要提供一个选项对象，格式与 [tls.createSecureContext](https://nodejs.org/api/tls.html#tls_tls_createsecurecontext_options) 相同。请注意，参数需要证书的字符串，而不是证书的文件名。这是一个简单的例子：

```js
// 加载模块
let mysql = require('mysql');
let fs = require('fs');

let conn = mysql.createConnection({
    host: 'localhost',
    ssl: {
        ca: fs.readFileSync(`${__dirname}/msyql-ca.crt`);
    }
});
```

当然，你可以通过设置不认证安全模块。但你不能这么做

```js
// 加载模块
let mysql = require('mysql');

let conn = mysql.createConnection({
    host: 'localhost',
    ssl: {
        // 不要这样做
        rejectUnauthorized: false
    }
});
```

# 关闭连接

我们有两种方式来关闭数据库连接。

#### 1.使用 `end()` 方法

```js
conn.end(function(err){
    // 处理回调
});
```

#### 2.使用 `destroy()` 方法

```js
// 没有回调
conn.destroy()
```

# 连接池

数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏。这项技术能明显提高对数据库操作的性能。

创建一个连接池：

```js
// 加载模块
let mysql = require('mysql');
let pool = mysql.createPool({
    connectionLimit: 10,
    host: 'localhost',
    user: 'username',
    password: 'yourpassword',
    database: 'mydb'
});

// 查询数据
pool.query('SELECT * FROM user;', function(err, res, rows){
    if(err){
        // 异常处理
        throw err;
    }else{
        // 处理查询数据
        console.log(rows);
    }
});
```

高阶用法：

```js
// 加载模块
let mysql = require('mysql');
let pool = mysql.createPool({
    connectionLimit: 10,
    host: 'localhost',
    user: 'username',
    password: 'yourpassword',
    database: 'mydb'
});

// 使用连接
pool.getConnection(function(err, conn){
    conn.query('SELECT * FROM user;', function(err, res, rows){
        // 完成连接后，将其释放
        conn.release();
        
        // 不要在这里使用连接，它已经返回到池中
        if(err){
            throw err;
        }else{
            console.log(rows);
        }
    });
});
```

#### 配置

| 选项               | 说明                                   | 默认值 |
| ------------------ | -------------------------------------- | ------ |
| acquireTimeout     | 连接获取期间发生超时前的毫秒数         | 10000  |
| waitForConnections | 确定没有可用连接且已达到限制时池的操作 | true   |
| connectionLimit    | 一次创建的最大连接数                   | 10     |
| queueLimit         | 池在返回错误之前将排队的最大连接请求数 | 0      |

#### 事件

**acquire**

> 当从连接池获得连接时，会触发此事件

```js
pool.on('acquire', function(conn){
    console.log('当前连接的线程ID是：%d', conn.threadId);
});
```

**connection**

> 当连接池建立新连接时，会触发此事件

```js
pool.on('connection', function(conn){
    conn.query('SET SESSION auto_increment_increment=1');
});
```

**enqueue**

> 当无可用连接时，会进入排队状态，此时会触发此事件

```js
pool.on('enqueue', function(){
    console.log('等待可用连接');
});
```

**release**

> 当连接被释放时，会触发此事件

```js
pool.on('release', function(conn){
    console.log('当前连接的线程(%d)已被释放', conn.threadId);
});
```

#### 关闭所有连接

完成使用池后，必须结束所有连接，否则 Node.js 事件循环将保持活动状态，直到 MySQL 服务器关闭连接。

```js
pool.end(function(err){
    // 处理回调
});
```

一旦 `pool.end` 被调用，`pool.getConnection` 就无法再执行其它操作。在调用 `pool.end` 之前，连接池中所有连接都被释放。

如果使用 `pool.query` 的快捷方式代替了 `pool.getConnection` -> `conn.query` -> `...`，连接池会等待，直到它完成。

`pool.end` 会调用连接池中每个活动连接的 `conn.end` 方法来关闭连接。每个将要关闭的连接被分配到一个 `QUIT` 队列，并设置一个标志来防止连接池创建新连接。正在进行的命令或查询都将会完成，但不会执行新命令。



# PoolCluster

`PoolCluster` 提供多主机集群连接。

```js
// 创建
let poolCluster = mysql.createPoolCluster();

// 添加配置
poolCluster.add(config); // 使用自动名称添加配置
poolCluster.add('MASTER', masterConfig); // 自定义名称添加配置
poolCluster.add('SLAVE1', slave1Config);

// 删除配置
poolCluster.remove('SLAVE1');
poolCluster.remove('SLAVE*'); // 支持通配符

// 目标集群：全部，选择器：round-robin(default)
poolCluster.getConnection(function(err, conn){
    // todo
});

// 目标集群：MASTER，选择器：round-robin(default)
poolCluster.getConnection('MASTER', function(err, conn){
    // todo
});

// 目标集群：SLAVE*，选择器：ORDER
poolCluster.getConnection('SLAVE*', 'ORDER', function(err, conn){
    // todo
});

// 目标集群：SLAVE[12]，选择器：ORDER
poolCluster.getConnection(/^SLAVE[12]$/, 'ORDER', function(err, conn){
    // todo
});

// 事件
poolCluster.on('remove', function(nodeId){
	console.log('REMOVED NODE : ' + nodeId); // nodeId = SLAVE1
});

// of namespace : of(pattern, selector)
poolCluster.of('*').getConnection(function(err, conn){
    // todo
});

let pool = poolCluster.of('SLAVE*', 'RANDOM');
pool.getConnection(function(err, conn){});
pool.getConnection(function(err, conn){});
pool.query(function(err, res, rows){});
 
// close all connections
poolCluster.end(function (err) {
  // all connections in the pool cluster have ended
});
```

#### 配置

| 选项                 | 说明                                                     | 默认值 |
| -------------------- | -------------------------------------------------------- | ------ |
| canRetry             | 连接失败时，是否尝试重新连接                             | true   |
| removeNodeErrorCount | 当连接错误数达到多少时，从集群中删除该不可用节点         | 5      |
| restoreNodeTimeout   | 如果连接失败，尝试重新连接的毫秒数                       | 0      |
| defaultSelector      | 默认选择器，RR-循环，RANDOM-随机，ORDER-第一个可用的节点 | RR     |

```js
let clusterConfig = {
  removeNodeErrorCount: 1, // Remove the node immediately when connection fails.
  defaultSelector: 'ORDER'
};
 
let poolCluster = mysql.createPoolCluster(clusterConfig);
```



#### 切换用户并改变连接状态

MySQL提供了一个changeUser命令，允许您在不关闭底层套接字的情况下更改当前用户和连接的其他方面：

```js
conn.changeUser({user : 'username'}, function(err){
	if(err){
        throw err;
    }else{
        // todo
    }
});
```

**参数说明**

| 参数     | 类型   | 说明                             |
| -------- | ------ | -------------------------------- |
| user     | string | 用户名默认为上一个用户           |
| password | string | 用户密码，默认为上一个用户的密码 |
| charset  | string | 字符集，默认为上一个字符集       |
| database | string | 数据库，默认为上一个数据库       |

# 服务器断开连接（待完善）

由于网络问题，服务器计时，服务器重新启动或崩溃，您可能会丢失与MySQL服务器的连接。所有这些事件都被认为是致命的错误，并且会有`err.code = 'PROTOCOL_CONNECTION_LOST'`。有关更多信息，请参阅[错误处理](https://www.npmjs.com/package/mysql#error-handling)部分。

通过建立新连接来重新连接连接。终止后，无法通过设计重新连接现有连接对象。

使用Pool时，将从池中删除断开连接的连接，释放空间，以便在下一次getConnection调用时创建新连接。

#　查询（待完善）

执行查询的最基本方法是调用`.query()`的对象上的方法（如`Connection`，`Pool`或`PoolNamespace`实例）。

**1.方法一：**

最简单的形式。`query()`是`.query(sqlString, callback)`，SQL字符串是第一个参数，第二个是回调：

```js
conn.query('SELECT * FROM `user` WHERE `name`="eysonyou";', function(err, res, rows){
    // todo
});
```

**2.方法二：**

`.query(sqlString, values, callback)`使用占位符值时会出现第二种形式（请参阅[转义查询值](https://www.npmjs.com/package/mysql#escaping-query-values)）：

```js
conn.query('SELECT * FROM `user` WHERE `name`=?;', ['eysonyou'], function(err, res, rows){
    // todo
});
```

**3.方法三：**

第三种形式`.query(options, callback)`是在查询中使用各种高级选项时出现的，例如[转义查询值](https://www.npmjs.com/package/mysql#escaping-query-values)， [连接重叠列名](https://www.npmjs.com/package/mysql#joins-with-overlapping-column-names)， [超时](https://www.npmjs.com/package/mysql#timeout)和[类型转换](https://www.npmjs.com/package/mysql#type-casting)。

```js
conn.query({
    sql: 'SELECT * FROM `user` WHERE `name`=?;',
    values: ['eysonyou'],
    timeout: 40000
}, function(err, res, rows){
    // todo
});
```

# 转义查询值（待完善）

**注意**这些转义值的方法仅在 禁用[NO_BACKSLASH_ESCAPES](https://dev.mysql.com/doc/refman/5.7/en/sql-mode.html#sqlmode_no_backslash_escapes) SQL模式时才有效 （这是MySQL服务器的默认状态）。

为了避免SQL注入攻击，在SQL查询中使用它之前，应始终转义任何用户提供的数据。你可以这样做使用`mysql.escape()`，`conn.escape()`或`pool.escape()`方法。

**1.拼接字符串**

```js
conn.query('SELECT * FROM `user` WHERE `name`="' + conn.escape('eysonyou') + '"', function(err, res, rows){
    // todo
});
```

**2.占位符**

或者，你可以使用 `?` 字符作为你想要转义的值的占位符，如下所示：

```js
conn.query('SELECT * FROM `user` WHERE `name`=?;', ['eysonyou'], function(err, res, rows){
    // todo
});
```

这看起来类似于MySQL中的预处理语句，但它实际上只是在`connection.escape()`内部使用相同的方法。

**注意**

这也与准备好的语句的不同之处在于所有语句都`?`被替换，即使是注释和字符串中包含的语句。

不同的值类型以不同方式进行转义，具体方法如下：

- 数字保持不变
- 布尔值转换为`true`/`false`
- 日期对象将转换为`'YYYY-mm-dd HH:ii:ss'`字符串
- 缓冲区转换为十六进制字符串，例如 `X'0fa5'`
- 字符串安全地逃脱
- 数组变成列表，例如`['a', 'b']`变成`'a', 'b'`
- 嵌套数组被转换为分组列表（对于批量插入），例如`[['a', 'b'], ['c', 'd']]`变成`('a', 'b'), ('c', 'd')`
- 具有`toSqlString`方法的对象将`.toSqlString()`被调用，并且返回的值将用作原始SQL。
- 对象被转换为对象上`key = 'val'`每个可枚举属性的对。如果属性的值是函数，则跳过它; 如果属性的值是一个对象，则在其上调用toString（）并使用返回的值。
- `undefined`/ `null`转换为`NULL`
- `NaN`/ `Infinity`保持原样。MySQL不支持这些，并且尝试将它们作为值插入将触发MySQL错误，直到它们实现支持。

**3.更好的方法**

```js
let post  = {id: 1, title: 'Hello MySQL'};
let query = conn.query('INSERT INTO posts SET ?', post, function(err, res, rows){
	if(error) throw error;
  	// Neat!
});

console.log(query.sql); // INSERT INTO posts SET `id` = 1, `title` = 'Hello MySQL'
```

# 转义查询标识符

如果您不能信任SQL标识符（数据库/表/列名称），因为它是由用户提供的，您应该使用它来转义它`mysql.escapeId(identifier)`， `conn.escapeId(identifier)`或者`pool.escapeId(identifier)`像这样：

```js
let sorter = 'date';
let sql = 'SELECT * FROM posts ORDER BY ' + conn.escapeId(sorter);

// 
// let sql = 'SELECT * FROM posts ORDER BY ' + conn.escapeId(sorter, true);
conn.query(sql, function(err, res, rows){
  	if(error) throw error;
});

let sql = 'SELECT * FROM posts ORDER BY ' + conn.escapeId('posts.' + sorter);
// -> SELECT * FROM posts ORDER BY `posts`.`date`
```

如果您不想将其`.`视为限定标识符，则可以将第二个参数设置`true`为以将字符串保留为文字标识符：

```js
let sorter = 'date.2';
let sql = 'SELECT * FROM posts ORDER BY ' + conn.escapeId(sorter, true);
// -> SELECT * FROM posts ORDER BY `date.2`
```

或者，您可以使用`??`字符作为您想要转义的标识符的占位符，如下所示：

```js
let userId = 1;
let columns = ['username', 'email'];
let query = conn.query('SELECT ?? FROM ?? WHERE id = ?', [columns, 'users', userId], function (err, res, rows) {
	if (error) throw error;
  	// ...
});
 
console.log(query.sql); // SELECT `username`, `email` FROM `users` WHERE id = 1
```

#### format

```js
let sql = "SELECT * FROM ?? WHERE ?? = ?";
let inserts = ['users', 'id', userId];
sql = mysql.format(sql, inserts);
```

#### 自定义 format

```js
conn.config.queryFormat = function(query, values){
  	if (!values) return query;
  	return query.replace(/\:(\w+)/g, function(txt, key){
    	if (values.hasOwnProperty(key)) {
      		return this.escape(values[key]);
    	}
    	return txt;
  	}.bind(this));
};
 
conn.query("UPDATE posts SET title = :title", { title: "Hello MySQL" });
```

#### 获取 insertId

```js
conn.query('INSERT INTO posts SET ?', {title: 'test'}, function (err, res, rows) {
  	if (err) throw err;
  	console.log(res.insertId);
});
```

#### 获取影响行数

```js
conn.query('DELETE FROM posts WHERE title = "wrong"', function (err, res, rows) {
  	if (err) throw err;
  	console.log('deleted ' + res.affectedRows + ' rows');
})
```

#### 修改行数

```js
conn.query('UPDATE posts SET ...', function (err, res, rows) {
  	if (err) throw err;
  	console.log('changed ' + res.changedRows + ' rows');
})
```

#### 获取连接ID

```js
conn.connect(function(err) {
  	if (err) throw err;
  	console.log('connected as id ' + conn.threadId);
});
```

#### 并行执行查询

MySQL协议是顺序的，这意味着您需要多个连接来并行执行查询。您可以使用池来管理连接，一种简单的方法是为每个传入的http请求创建一个连接。

#### 流式查询行

有时您可能希望选择大量的行并在收到它们时处理每个行。这可以这样做：

```js
let query = conn.query('SELECT * FROM posts');
query
  .on('error', function(err) {
    // Handle error, an 'end' event will be emitted after this as well
  })
  .on('fields', function(rows) {
    // the field packets for the rows to follow
  })
  .on('result', function(res) {
    // Pausing the connnection is useful if your processing involves I/O
    conn.pause();
 
    processRow(res, function() {
      conn.resume();
    });
  })
  .on('end', function() {
    // all rows have been received
  });
```

请注意以上示例的一些内容：

- 通常，在开始使用连接节流之前，您需要接收一定数量的行`pause()`。此数字取决于行的数量和大小。
- `pause()`/ `resume()`操作底层套接字和解析器。您可以保证`'result'`在致电后不会再发生任何事件`pause()`。
- `query()`在流式传输时，您不能提供对该方法的回调。
- 该`'result'`事件将触发两行以及确认INSERT / UPDATE查询成功的OK数据包。
- 非常重要的是不要将结果暂停太长时间，否则您可能会遇到`Error: Connection lost: The server closed the connection.` 此时间限制由 MySQL服务器上的[net_write_timeout设置](https://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_net_write_timeout)决定 。

此外，您可能有兴趣知道目前无法流式传输单个行列，它们将始终完全缓冲。如果你有一个很好的用例来传输MySQL的大型字段，我很想得到你的想法和贡献。

#### 使用Streams管道结果

查询对象提供了一种便捷方法`.stream([options])`，可将查询事件包装到[可读流](http://nodejs.org/api/stream.html#stream_class_stream_readable) 对象中。该流可以很容易地通过管道传输到下游，并根据下游拥塞和可选项提供自动暂停/恢复`highWaterMark`。`objectMode`流的参数设置为`true`且无法更改（如果需要字节流，则需要使用变换流，例如 [objstream](https://www.npmjs.com/package/objstream)）。

例如，将查询结果传递到另一个流（最大缓冲区为5个对象）就是：

```js
conn.query('SELECT * FROM posts')
  .stream({highWaterMark: 5})
  .pipe(...);
```

# 多语句查询

出于安全原因，禁用对多个语句的支持（如果未正确转义值，则允许SQL注入攻击）。要使用此功能，您必须为连接启用它：

```js
let conn = mysql.createConnection({multipleStatements: true});
```

启用后，您可以像执行任何其他查询一样执行多个语句查询：

```js
conn.query('SELECT 1; SELECT 2', function (err, res, rows) {
  	if (err) throw err;
  	// `results` is an array with one element for every statement in the query:
  	console.log(res[0]); // [{1: 1}]
  	console.log(res[1]); // [{2: 2}]
});
```

此外，您还可以流式传输多个语句查询的结果：

```js
var query = connection.query('SELECT 1; SELECT 2');
 
query
  .on('fields', function(fields, index) {
    // the fields for the result rows that follow
  })
  .on('result', function(row, index) {
    // index refers to the statement this result belongs to (starts at 0)
  });
```

# 存储过程

你可以在你的查询语句里面调用MySQL驱动中自带的任何存储过程，如果你使用存储过程生成的多个结果集，其实也就与您使用多语句查询生成得出的结果是一样的。

# 合并重叠的字段

当我们使用JOIN函数执行查询的时候得到的结果里面有很多字段是重复的。默认情况下Node-MySQL会按照列读取顺序把一些冲突的列名进行合并。但是这样有可能会导致一些接收到的值变得不可用。
不过，您可以指定在表名中嵌套您需要的列，就像这样：

```js
var options = {sql: '...', nestTables: true};
connection.query(options, function (error, results, fields) {
  if (error) throw error;
  /* results will be an array like this now:
  [{
    table1: {
      fieldA: '...',
      fieldB: '...',
    },
    table2: {
      fieldA: '...',
      fieldB: '...',
    },
  }, ...]
  */
});
```

或者你可以使用字符串分隔符合并成你想要的结果:

```js
var options = {sql: '...', nestTables: '_'};
connection.query(options, function (error, results, fields) {
  if (error) throw error;
  /* results will be an array like this now:
  [{
    table1_fieldA: '...',
    table1_fieldB: '...',
    table2_fieldA: '...',
    table2_fieldB: '...',
  }, ...]
  */
});
```

# 事务处理

在连接层中可以支持简单的事务处理：

```js
connection.beginTransaction(function(err) {
  if (err) { throw err; }
  connection.query('INSERT INTO posts SET title=?', title, function (error, results, fields) {
    if (error) {
      return connection.rollback(function() {
        throw error;
      });
    }
 
    var log = 'Post ' + results.insertId + ' added';
 
    connection.query('INSERT INTO log SET data=?', log, function (error, results, fields) {
      if (error) {
        return connection.rollback(function() {
          throw error;
        });
      }
      connection.commit(function(err) {
        if (err) {
          return connection.rollback(function() {
            throw err;
          });
        }
        console.log('success!');
      });
    });
  });
});
```

请注意，执行START TRANSACTION, COMMIT,和 ROLLBACK 这些简单命令的方法分别是transaction(),Commit()和rollback(),了解MySQL中一些会造成隐式提交语句很重要。

# Ping

可以使用该`connection.ping`方法通过连接发送ping数据包。此方法将ping数据包发送到服务器，当服务器响应时，将触发回调。如果发生错误，将使用错误参数触发回调。

```js
connection.ping(function (err) {
  if (err) throw err;
  console.log('Server responded to ping');
})
```

# 超时

每个操作都采用可选的非活动超时选项。这允许您为操作指定适当的超时。值得注意的是，这些超时不是MySQL协议的一部分，而是通过客户端进行超时操作。这意味着当达到超时时，它所发生的连接将被破坏，并且不能执行进一步的操作。

```js
// Kill query after 60s
connection.query({sql: 'SELECT COUNT(*) AS count FROM big_table', timeout: 60000}, function (error, results, fields) {
  if (error && error.code === 'PROTOCOL_SEQUENCE_TIMEOUT') {
    throw new Error('too long to count table rows!');
  }
 
  if (error) {
    throw error;
  }
 
  console.log(results[0].count + ' rows');
});
```

# 错误处理

此模块提供了一致的错误处理方法，您应仔细查看以编写可靠的应用程序。

此模块创建的大多数错误都是JavaScript [Error](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Error) 对象的实例。此外，他们通常还有两个额外的属性：

- `err.code`：[MySQL服务器错误](http://dev.mysql.com/doc/refman/5.5/en/error-messages-server.html)（例如 `'ER_ACCESS_DENIED_ERROR'`），Node.js错误（例如`'ECONNREFUSED'`）或内部错误（例如`'PROTOCOL_CONNECTION_LOST'`）。
- `err.fatal`：Boolean，指示此错误是否为连接对象的终端。如果错误不是来自MySQL协议操作，则不能正确定义。
- `err.sql`：String，包含失败查询的完整SQL。当使用更高级别的接口（如生成查询的ORM）时，这非常有用。
- `err.sqlState`：String，包含五个字符的SQLSTATE值。仅填充[MySQL服务器错误](http://dev.mysql.com/doc/refman/5.5/en/error-messages-server.html)。
- `err.sqlMessage`：String，包含提供错误的文本描述的消息字符串。仅填充[MySQL服务器错误](http://dev.mysql.com/doc/refman/5.5/en/error-messages-server.html)。

致命错误会传播到*所有*待处理的回调。在下面的示例中，尝试连接到无效端口会触发致命错误。因此，错误对象将传播到两个挂起的回调：

```js
var connection = require('mysql').createConnection({
  	port: 84943, // WRONG PORT
});
 
connection.connect(function(err) {
  	console.log(err.code); // 'ECONNREFUSED'
  	console.log(err.fatal); // true
});
 
connection.query('SELECT 1', function (error, results, fields) {
  	console.log(error.code); // 'ECONNREFUSED'
  	console.log(error.fatal); // true
});
```

但是，正常错误仅委托给它们所属的回调。因此，在下面的示例中，只有第一个回调收到错误，第二个查询按预期工作：

```js
connection.query('USE name_of_db_that_does_not_exist', function (error, results, fields) {
  	console.log(error.code); // 'ER_BAD_DB_ERROR'
});
 
connection.query('SELECT 1', function (error, results, fields) {
  	console.log(error); // null
  	console.log(results.length); // 1
});
```

最后但并非最不重要的：如果发生致命错误并且没有挂起的回调，或者发生了没有属于它的回调的正常错误，则错误将作为`'error'`连接对象上的事件发出。这在以下示例中进行了演示：

```js
connection.on('error', function(err) {
  	console.log(err.code); // 'ER_BAD_DB_ERROR'
});
 
connection.query('USE name_of_db_that_does_not_exist');
```

注意：`'error'`事件在节点中是特殊的。如果它们在没有附加侦听器的情况下发生，则会打印堆栈跟踪并终止您的进程。

**tl; dr：**这个模块不希望你处理静默失败。您应始终为方法调用提供回调。如果要忽略此建议并禁止未处理的错误，可以执行以下操作：

```js
// I am Chuck Norris:
connection.on('error', function() {});
```

# 例外安全

为方便起见，此驱动程序默认情况下会将mysql类型转换为本机JavaScript类型。存在以下映射：

### 数

- TINYINT
- SMALLINT
- INT
- MEDIUMINT
- 年
- 浮动
- 双

### 日期

- TIMESTAMP
- 日期
- 约会时间

### 缓冲

- TINYBLOB
- MEDIUMBLOB
- LONGBLOB
- BLOB
- BINARY
- VARBINARY
- BIT（必要时，最后一个字节将填充0位）

### 串

二进制字符集中的**注释**文本将返回`Buffer`，而不是字符串。

- CHAR
- VARCHAR
- TINYTEXT
- MEDIUMTEXT
- LONGTEXT
- 文本
- ENUM
- 组
- DECIMAL（可能超过浮动精度）
- BIGINT（可能超过浮动精度）
- TIME（可以映射到Date，但是会设置什么日期？）
- 几何（从未使用过，如果你这样做，请联系）

不建议（并且可能会在将来消失/更改）禁用类型转换，但您目前可以在连接上执行此操作：

```js
var connection = require('mysql').createConnection({typeCast: false});
```

或者在查询级别：

```js
var options = {sql: '...', typeCast: false};
var query = connection.query(options, function (error, results, fields) {
  if (error) throw error;
  // ...
});
```

您也可以自己传递函数并处理类型转换。您将获得一些列信息，如数据库，表和名称，以及类型和长度。如果您只想将自定义类型转换应用于特定类型，则可以执行此操作，然后回退到默认类型。这是转换`TINYINT(1)`为布尔值的示例：

```js
connection.query({
  sql: '...',
  typeCast: function (field, next) {
    if (field.type == 'TINY' && field.length == 1) {
      return (field.string() == '1'); // 1 = true, 0 = false
    }
    return next();
  }
});
```

**警告：您必须使用自定义typeCast回调中的这三个字段函数之一来调用解析器。它们只能被调用一次。（参见＃539讨论）**

```js
field.string()
field.buffer()
field.geometry()
```

是别名

```js
parser.parseLengthCodedString()
parser.parseLengthCodedBuffer()
parser.parseGeometryValue()
```

**您可以通过查看以下内容找到需要使用的字段函数：RowDataPacket.prototype._typeCast**

# 连接标志

如果,由于某些原因你需要修改默认的连接标记,那么你可能需要使用flags选项。通过在连接配置的选项列表中添加这个选项，那么你就可以修改默认的连接标记.如果你不想使用默认的flag你可以使用一个减号来取消掉。或者在连接的配置选项列表里面添加一个flag选项，而不写值，仅仅就是一个flag名字在哪里（不区分大小写）。
 PS:不想像去掉flag选项一样在flag的前面加一个+号：

> 请注意：有些默认的flag目前还不支持（例如：SSL,Compression）。

例子：
 下面的例子是使用黑名单FOUND_ROWS flag例子：

```js
var connection = mysql.createConnection("mysql://localhost/test?flags=-FOUND_ROWS");
```

### 默认标志

默认情况下，在新连接上发送以下标志：

- `CONNECT_WITH_DB` - 能够在连接时指定数据库。
- `FOUND_ROWS`- 发送找到的行而不是受影响的行`affectedRows`。
- `IGNORE_SIGPIPE` - 老; 没有效果。
- `IGNORE_SPACE`- 让解析器在`(`in查询之前忽略空格。
- `LOCAL_FILES`- 可以使用`LOAD DATA LOCAL`。
- `LONG_FLAG`
- `LONG_PASSWORD` - 使用改进版的旧密码验证。
- `MULTI_RESULTS` - 可以为COM_QUERY处理多个结果集。
- `ODBC`旧; 没有效果。
- `PROTOCOL_41` - 使用4.1协议。
- `PS_MULTI_RESULTS` - 可以处理COM_STMT_EXECUTE的多个结果集。
- `RESERVED` - 4.1协议的旧标志。
- `SECURE_CONNECTION` - 支持本机4.1身份验证。
- `TRANSACTIONS` - 询问交易状态标志。

此外，如果选项`multipleStatements` 设置为以下，将发送以下标志`true`：

- `MULTI_STATEMENTS` - 客户端可以为每个查询或语句准备发送多个语句。

### 其他可用的标志

还有其他标志可用。它们可能会或可能不会起作用，但仍可指定。

- `COMPRESS`
- `INTERACTIVE`
- `NO_SCHEMA`
- `PLUGIN_AUTH`
- `REMEMBER_OPTIONS`
- `SSL`
- `SSL_VERIFY_SERVER_CERT`

# 调试和报告问题

如果您遇到问题，可能有用的一件事是启用 `debug`连接模式：

```js
var connection = mysql.createConnection({debug: true});
```

这将在stdout上打印所有传入和传出的数据包。您还可以通过将类型数组传递给debug来限制对数据包类型的调试：

```js
var  connection  = mysql 。createConnection （{ debug ：[ ' ComQueryPacket '，' RowDataPacket ' ] } ） ;   
```

限制对查询和数据包的调试。

如果这没有帮助，请随意打开GitHub问题。一个好的GitHub问题将有：

- 重现问题所需的最少代码量（如果可能）
- 您可以收集尽可能多的调试输出和有关您的环境的信息（mysql版本，节点版本，操作系统等）。

# 安全问题

安全问题不应首先通过GitHub或其他公共论坛报告，而应保密，以便协作者评估报告，并且（a）设计修订并计划发布日期或（b）断言它不是安全性问题（在这种情况下，它可以发布在公共论坛，如GitHub问题）。

主要的私人论坛是电子邮件，可以通过向模块的作者发送电子邮件或打开GitHub问题，只是询问应该向谁提出安全问题，而不会泄露问题或问题类型。

理想的报告将包括明确指出安全问题是什么以及如何利用它，理想情况是伴随着概念验证（“PoC”），让合作者再次工作并验证潜在的修复。

# 运行测试

测试套件分为两部分：单元测试和集成测试。单元测试在任何机器上运行，而集成测试需要设置MySQL服务器实例。

**单元测试**

```shell
$ FILTER=unit npm test
```

**集成测试**

```shell
$ mysql -u root -e "CREATE DATABASE IF NOT EXISTS node_mysql_test"
$ MYSQL_HOST=localhost MYSQL_PORT=3306 MYSQL_DATABASE=node_mysql_test MYSQL_USER=root MYSQL_PASSWORD= FILTER=integration npm test
```

