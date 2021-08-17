# melondb
### 简单、方便、快速、强大、神奇的数据库引擎

通过http连接数据库，可以在任何语言和框架中轻松调用，通过提交js指令（事务）操作数据库，无需考虑事务锁问题。

```
db.init()  //数据库初始化（新建或重置数据库）
db.save()  //数据库持久化（数据写入硬盘）
db.load()  //重新加载磁盘数据到内存（忽略未保存数据）
```

```
// 在nodejs中可以通过fetch连接数据库

const fetch=require("node-fetch");
async function dbc(s){
	let t="test"; //数据库名称，可自定义
	return fetch("http://127.0.0.1:1688/",{
		method: "POST",
		body: JSON.stringify({t,s})
	}).then(res=>res.text());
}
async function main(){
	// await dbc(`db.init()`); //db.init()为数据库初始化，只需执行一次
	let obj=`{name:"Candy",age:25}`;
	await dbc(`db.user=${obj}`);
	let res=await dbc(`db.count=db.count||0; db.count++; return db.count;`);
	console.log("res", res);
}
main();
```

```
// 在go中使用

package main
import (
	"fmt"
	"github.com/caixiaogua/melon-db/melondb"
)
func main() {
	dbc := melondb.Init("test", "http://127.0.0.1:1688/")
	res := dbc(`db.init(); db.arr=[1,2,3,"nice"]; return db;`) //db.init()为数据库初始化，仅需执行一次
	fmt.Println("res", res)
}
```

```
// 在jsgo中使用

function main(){
	var s=JSON.stringify({
		t:"test", //数据库名称，可自定义
		s:"db.init(); db.arr=[1,2,3]; return db" //db.init()为数据库初始化，仅需执行一次
	})
	var res=api.httpPost("http://127.0.0.1:1688/", s, "");
	return res;
}
```
