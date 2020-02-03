
项目
----

安装
^^^^

.. code-block:: text

   $ php artisan system:install
   $ php artisan system:user create_user

项目目录树
^^^^^^^^^^

.. code-block:: text

   ├── config              # 配置文件
   ├── extensions          # 扩展目录
   ├── modules             # 模块名称, 这里以system 模块 作为说明, 详细见 模块目录树
   │   ├── finance
   │   ├── system
   │   └── ...
   ├── node_modules        # node 模块
   ├── public
   │   ├── docs            # 文档
   │   ├── modules         # 模块资源
   │   └── resources       # 资源项目 / 公共
   │       ├── css
   │       ├── js
   │       └── scss
   ├── resources           # 资源源文件
   │   ├── assets
   │   ├── centrifugo      # socket 服务端
   │   ├── docs
   │   ├── lang
   │       ├── en
   │       └── zh
   ├── storage             # 存储目录
   │   ├── app             # 应用资源
   │   ├── bootstrap       # 启动项目
   │   ├── bower           # bower 文档
   │   ├── clockwork       # 调试文件
   │   ├── console         # 控制器
   │   ├── framework       # 框架缓存
   │   │   ├── cache
   │   │   ├── sessions
   │   │   └── views
   │   ├── logs            # 日志
   │   ├── phplint
   │   ├── purifier
   │   ├── sami
   ├── tests               # 测试目录
   │   ├── MerchantApi
   │   │   └── User
   │   └── WebApi
   │       └── User
   └── vendor              # 第三方文档(只是预览, 不做详细说明)


单元测试
^^^^^^^^^^

.. code-block:: text

   首先安装PHPunit  PHPunit --version 可以查看是否安装和PHPunit版本
   PHPstorm配置:
    - 在setting 搜索PHP 设置php版本
    - 在php下 Test Frameworks 中设置 phpunit路径
    - default bootstrap中设置框架自动加载文件目录
   使用本框架的  php artisan poppy:test  模块名 测试文件名来创建测试文件
