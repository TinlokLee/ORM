###   1 项目中的所有数据（用户信息，日志，评论等）都储存在MySQL
###   2 访问数据库需要创建：数据库连接，游标对象，执行 SQL 语句，异常处理，资源清理
####     所以，把常用的SELECT，INSERT，UPTARE，DELETE操作函数封装，便于复用

###   3 web框架使用基于协程的异步asyncio模型，aiohttp
####     异步编程原则：一旦使用异步，系统的每一层都必须是异步
####     MySQL数据库的异步 IO 驱动由aiomysql提供

###    开发流程：
####   1 创建一个全局连接池
####     由全局变量__pool存储，自动提交事务，缺省情况下编码设置为 utf-8
####   2 封装 select/insert/update/delete 函数
####     防止SQL注入：始终使用带参的 SQL
####     使用到的方法：cur.fetchmany() 获取最多指定数量的记录
####                  cur.fetchall()  获取所有记录
####                  cur.rowcount    返回结果数
###    3 编写 ORM
####     1) 首先定义 User 对象，然后把数据库表 users 和它关联起来
####        __table__ = 'users'  id作为 primary_key
####        User 实例存入数据库 await user.save() 异步实现
####     2) 定义Model
####        首先定义所有 ORM 映射的基类 Model
####        class Model(dict, metaclass=ModelMetaclass) 从dict 继承，具备其功能
####        使用方法：__init__()    __getattr__()     __setattr__()
####                 super().__init__()
####                 @classmethod 所有子类可调用class 方法
####                 添加实例方法，使所有子类可调用实例方法
####     3) 定义Field 和各种 Field 子类
####        字段映射使用： ddl='varchar()'
####     4) 编写元类
####        映射具体子类如User 的信息读取出来
####        使用方法： __new__() 
####                 获取所有table 名称，Field, 主键名
####                 遍历items()，然后判断v 与 Field，primarykey关系
####                 保存属性和列的映射关系 __mappings__
####                 构造默认的 select/insert/update/delete 语句
####        




