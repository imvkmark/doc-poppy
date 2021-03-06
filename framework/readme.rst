README
======

.. image:: https://badges.gitter.im/poppy-framework/Lobby.svg
   :target: https://gitter.im/poppy-framework/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge
   :alt: Join the chat at https://gitter.im/poppy-framework/Lobby


Agamotto
--------

Agamotto是DateTime的一个简单的PHP API扩展，是Carbon的亲戚, 这个库的目标是创建一个显示本地化日期的解决方案
并引入更健壮的特性。

这个库是Date库的镜像名称:

`Date <https://github.com/jenssegers/date>`_
`Carbon <https://github.com/briannesbitt/carbon>`_

命令行/Console
--------------

创建模块
^^^^^^^^

创建一个 Poppy 模块并启动它.

.. code-block:: text

   $ php artisan poppy:make {slug} [-Q|--quick]

模块文件树:

.. code-block:: text

   ├── configurations        # 配置文件
   ├── docs                  # 文档
   ├── resources
   │   ├── lang              # 语言文件
   │   │   └── zh            # 语言文件夹
   │   └── views             # blade 模板
   └── src
       ├── classes
       ├── database
       │   ├── factories
       │   ├── migrations
       │   └── seeds
       ├── events
       ├── http
       │   ├── request
       │   │   ├── api
       │   │   ├── backend
       │   │   └── web
       │   └── routes
       ├── listeners
       └── models

列出 Modules
^^^^^^^^^^^^

列出所有的应用模块

.. code-block:: text

   $ php artisan poppy:list

   +------+--------+--------+-------------------------------------------------------+---------+
   | #    | Name   | Slug   | Description                                           | Status  |
   +------+--------+--------+-------------------------------------------------------+---------+
   | 9001 | System | system | This is the description for the poppy Backend module. | Enabled |
   | 9001 | Slt    | slt    | Sour Lemon Team                                       | Enabled |
   +------+--------+--------+-------------------------------------------------------+---------+

启用/禁用模块
^^^^^^^^^^^^^

.. code-block:: text

   $ php artisan poppy:enable {slug}
   $ php artisan poppy:disable {slug}

优化模块
^^^^^^^^

模块优化, 清空生成的缓存等操作

.. code-block:: text

   $ php artisan poppy:optimize

Poppy 数据库管理
^^^^^^^^^^^^^^^^

.. code-block:: text

   poppy:migrate           执行模块的数据库迁移文件
   poppy:migrate:refresh   重新执行模块数据库迁移文件
   poppy:migrate:reset     回滚所有执行的数据库迁移
   poppy:migrate:rollback  回滚执行完的上一个数据库迁移
   poppy:migration {slug}  创建一个指定模块的数据库迁移文件

Poppy 生成器
------------

生成器工具

.. code-block:: text

   php artisan poppy:command {slug} {name}
   php artisan poppy:controller {slug} {api/web} {name}
   php artisan poppy:middleware {slug} {name}
   php artisan poppy:model {slug} {name}
   php artisan poppy:policy {slug} {name}
   php artisan poppy:provider {slug} {name}
   php artisan poppy:request {slug} {name}
   php artisan poppy:seed {slug} {name}
   php artisan poppy:seeder {slug} {name}
   php artisan poppy:test {slug} {name}

生成命令文件
^^^^^^^^^^^^

.. code-block:: text

   php artisan poppy:command {slug} {name}

生成控制器文件
^^^^^^^^^^^^^^

.. code-block:: text

   php artisan poppy:controller {slug} {api/web} {name}

生成中间件文件
^^^^^^^^^^^^^^

.. code-block:: text

   php artisan poppy:middleware {slug} {name}

生成数据库模型文件
^^^^^^^^^^^^^^^^^^

.. code-block:: text

   php artisan poppy:model {slug} {name}

生成policy策略文件
^^^^^^^^^^^^^^^^^^

.. code-block:: text

   php artisan poppy:policy {slug} {name}

生成服务提供者provider文件
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: text

   php artisan poppy:provider {slug} {name}

生成request文件
^^^^^^^^^^^^^^^

.. code-block:: text

   php artisan poppy:request {slug} {name}

生成种子文件
^^^^^^^^^^^^

.. code-block:: text

   php artisan poppy:seeder {slug} {name}

写入种子
^^^^^^^^

.. code-block:: text

   php artisan poppy:seed

生成测试文件
^^^^^^^^^^^^

.. code-block:: text

   php artisan poppy:test

事件
----

.. code-block:: text

   // Locale Changed
   Events\LocaleChanged($locale)

   // Module Maked
   Events\PoppyMake($slug)

Helpers
-------

.. code-block:: text

   ArrayHelper
   CacheHelper
   ContentHelper
   EnvHelper
   FileHelper
   HtmlHelper
   ImageHelper
   CookieHelper
   RouterHelper
   SearchHelper
   StrHelper
   TimeHelper
   TreeHelper
   UtilHelper
   WebHelper

解析器
------

支持 Xml,Ini,Yaml

Blade 语法
----------

.. code-block:: text

   @poppy
   // You Can check if module is exist and enabled.
   @endpoppy

验证
----

.. code-block:: text

   1.语言包
    - resource/lang/zh/validator.php
   2.相应的验证规则
    - Rule 类
   3.FrameworkServiceProvider.php 验证正则规则

鸣谢 & Docs
-----------


* `Yaml <http://nodeca.github.io/js-yaml/>`_
* `EloquentFilter <https://github.com/Tucker-Eric/EloquentFilter>`_
* `Sami <https://github.com/FriendsOfPHP/Sami>`_


-  `EloquentFilter - 数据库过滤器 <https://github.com/Tucker-Eric/EloquentFilter/wiki>`_
-  `Carbon - 时间组件 <https://segmentfault.com/a/1190000014239090>`_
-  `Laravel Html & Form - Html/Form 封装 <https://segmentfault.com/a/1190000011580448>`_
-  `hieu-le/active-Url状态组件 <https://laravel-china.org/topics/2858/extended-recommendation-hieu-leactive-according-to-the-url-generated-corresponding-navigation-active-state>`_
-  `hashids/hashids - 对 ID 进行 Hash 加密 <https://github.com/vinkla/laravel-hashids>`_
