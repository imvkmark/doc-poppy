
前端(Laravel Mix)
-----------------

系统后台集成 layui 作为后台UI界面, 元素的使用详见 www.layui.com

前台页面开发
^^^^^^^^^^^^

前台页面依赖于 laravel-mix 来进行页面的开发. 下面会对以下的几个部分进行深入解析, 首先,我们先看下如何进行页面的布局和显示
常用文档 : `资源任务编译器 Laravel Mix <https://laravel-china.org/docs/laravel/5.5/mix/1307>`_

文件的位置
~~~~~~~~~~

.. code-block:: text

   # sass 源文件的位置, 使用的样式文件是 web.scss
   ~/resources/assets/scss/

   # 页面资源放置位置
   ~/module/resources/images

   # 监听文件位置, 这里使用的组件是 laravel-mix , 详细文档查看 : https://github.com/JeffreyWay/laravel-mix
   ~/webpack.mix.js

   # 生成 css 的位置
   ~/public/assets/css/

   # 图片的位置
   ~/public/modules/{user|system|web}/images/

   # 访问地址, 这里 {page} 代表需要布局的页面的名称
   # 例如 : http://dev.play.com/develop/l/layout.m.homepage
   http://{domain}/develop/l/{page}

   # 页面命名, 页面命名为 `{page}.blade.php`, 这里支持多层文件夹
   ~/resources/views

如何运行
~~~~~~~~

.. code-block:: text

   # 安装 
   $ npm install 

   # 开始监听 scss, 写的 css 
   $ npm run watch

apidoc 接口文档
^^^^^^^^^^^^^^^

apidoc 是一个简单的 RESTful API 文档生成工具，它从代码注释中提取特定格式的内容，生成文档。

.. code-block:: text

   php artisan system:doc api

配置信息

.. code-block:: text

   'apidoc' => [
       // key : 标识
       'web' => [
           // 标题
           'title'       => '后台接口',
           // 源文件夹
           'origin'      => 'modules',
           // 接口测试构建器
           'factory'     => \Site\Testing\WebApiFactory::class,
           // 生成目录
           'doc'         => 'public/docs/backend',
           // 默认访问的url
           'default_url' => 'api_v1/backend/system/role/permissions',
       ],
   ],
