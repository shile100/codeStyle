# Nodejs后端规约

## 项目目录 
```
  .
  ├── src  源码文件夹
  │   ├── app.js  应用启动入口
  │   ├── controller 接口参数校验及返回值
  │   ├── models  数据库ORM
  │   ├── middleware  中间件
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
 
## Controller 主要职责 检查请求参数 处理返回值格式
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
 
## Service 主要职责 封装逻辑代码
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
## Model ORM 数据库关系映射
### `用户model`
```
const modelName = "user";
const schema = new mongoose.Schema({
  owner: { type: mongoose.Schema.Types.ObjectId, ref: "user" },
  state: { type: String, enum: ["wrong", "right"], default: "wrong" },
  count: {type: Number, default: 1},
  question: { type: mongoose.Schema.Types.ObjectId, ref: "qa_question" }
});
module.exports = mongoose.model(modelName, schema);
```
## Router 定义暴露接口
```
const qaRouter = express.Router();
qaRouter.use(express.json());
qaRouter.use(express.urlencoded({ extended: true }));
qaRouter.get("/users", authenticate("jwt"), UserController.findUsers);
module.exports = qaRouter;
```
## Middleware 中间件层
### `错误处理中间件`
```
function errorHandler() {
  return function (error, req, res, next) {
    // 401
    if (error.type === "FeathersError" && error.name === "NotAuthenticated") {
      res.status(401).send({
        error: 1,
        message: error ? error.message : "未知错误"
      });
    } else {
      // 500
      res.send({
        error: 1,
        message: error ? error.message : "未知错误"
      });
    }
  };
};
module.exports = errorHandler;
```
## Repository 如果使用mongoose无需创建
