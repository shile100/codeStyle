# README 
 
## 项目目录 
```
  .
  ├── src  源码文件夹
  │   ├── app.js  应用启动入口
  │   ├── controller 接口参数校验及返回值
  │   ├── models  数据库ORM
  │   ├── router  路由文件
  │   ├── repository  数据库交互
  │   └── services  逻辑代码
  └── test  单元测试
      └── services 逻辑测试
```
## 接口定义规范 

### `用户接口定义`

- `/users [GET]` : 获取用户列表
- `/user  [POST]` : 新增用户
- `/user/:id [GET]` : 根据用户id获取用户
- `/user/:id [PUT]` : 根据用户id修改用户
- `/user/:id  [DELETE]` : 根据用户id删除用户

### `订单接口定义`

- `/orders?user_id=1 [GET]` : 根据用户id获取订单列表
- `/order  [POST]` : 新增订单
- `/order/:id [GET]` : 根据订单id获取订单
 
## Controller 
### `用户Controller`
```
class UserController {
  /**
   * 获取用户列表
   * @param {*} req 
   * @param {*} res 
   */
  static async findUsers(req, res) {
    const query = req.query || {};
    // 校验参数
    if (!query.id) {
      throw new Error("参数错误: id不存在");
    }
    // 逻辑调用
    const result = UserService.findUsers(query.id);
    // 返回值处理
    res.send({
      error: 0,
      data: result
    });
  }
  static async createUser(req, res) {
    
  }
  static async findUserById(req, res) {

  }

  static async UpdateUserById(req, res) {

  }
  static async RemoveUserById(req, res) {

  }
}
```
 
## Service
### `用户Service`
```
class UserService {
  /**
   * 登录逻辑
   * @param {string} username 用户名
   * @param {string} password 密码
   */
  static async login(username, password) {
    const user = await UserRepository.findUserByUsername(username);
    // 逻辑判断
    if(user.password === password) {
      return true;
    }else {
      return false;
    }
  }
}
```
## Repository
## Model
## Router

