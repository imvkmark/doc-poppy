
模块目录树
----------

.. code-block::

   ├── configurations       # 配置文件
   │   ├── pages.yaml
   │   ├── permissions.yaml
   ├── docs                 # 技术文档
   ├── resources            # 资源文件
   │   ├── config           # 发布的配置文件
   │   ├── lang             # 多语言支持包的位置
   │   │   └── zh
   │   ├── mixes            # mixs : 混杂的文件
   │   │   └── poppy        # poppy 核心包 (普通页面布局)
   │   │       └── backend
   │   ├── scss             # sass 预处理文件 (普通页面布局需要)
   │   │   ├── backend      
   │   │   ├── basic
   │   │   └── develop
   │   └── views            # 视图文件
   │       ├── backend      # 视图 - 后台
   │       ├── develop      # 视图 - 开发者平台
   │       ├── email        # 视图 - email
   │       └── tpl          # 视图 - 核心模板
   ├── src
   │   ├── action           # action
   │   ├── backend          # 后端模块管理
   │   ├── classes          # 基础类
   │   │   ├── auth         # 权限
   │   │   │   ├── guard    # 权限 - guard
   │   │   │   └── provider # 权限 - provider
   │   │   ├── payment      # 支付
   │   │   └── traits       # traits
   │   ├── commands         # 命令
   │   ├── database         # 数据库
   │   │   ├── factories    # 数据库 - 工厂
   │   │   ├── migrations   # 数据库 - migrations
   │   │   └── seeds        # 数据库 - 种子
   │   ├── events           # 事件, 事件使用 Event 后缀
   │   ├── extension        # 扩展包管理(开发用, 以后可能会废弃掉)
   │   ├── http             # 路由和中间件
   │   │   ├── middleware
   │   │   └── routes
   │   ├── listeners        # 事件 - 监听器
   │   ├── mail             # 邮件
   │   ├── models           # 模型
   │   │   ├── filters      # 模型 - 过滤器
   │   │   ├── policies     # 模型 - 策略
   │   │   └── resources    # 模型 - 资源
   │   ├── module           # 模块
   │   │   ├── listeners
   │   │   └── repositories
   │   ├── notifications    # 通知
   │   ├── permission       # 权限
   │   │   ├── commands
   │   │   ├── exceptions
   │   │   ├── middlewares
   │   │   └── repositories
   │   ├── rbac             # rbac 角色包
   │   │   ├── contracts 
   │   │   ├── facades
   │   │   ├── helper
   │   │   ├── middleware
   │   │   └── traits
   │   ├── request          # 控制器
   │   │   ├── api_v1       # 控制器 - Api
   │   │   │   ├── backend
   │   │   │   ├── pam
   │   │   │   └── util
   │   │   ├── backend      # 后端页面
   │   │   ├── develop      # 开发平台
   │   │   └── system       # 后台系统入口
   │   └── setting          # 系统设置
   └── tests                # 单元测试
