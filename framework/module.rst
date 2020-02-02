
模块(Module)
------------

功能开发流程
^^^^^^^^^^^^

.. code-block:: text

   明白需求 --> 拆分功能点 (整理逻辑) --> 写代码 --> 完成功能

单元测试
--------

.. code-block:: text

   首先安装PHPunit  PHPunit --version 可以查看是否安装和PHPunit版本
   PHPstorm配置:
    - 在setting 搜索PHP 设置php版本
    - 在php下 Test Frameworks 中设置 phpunit路径
    - default bootstrap中设置框架自动加载文件目录
   使用本框架的  php artisan poppy:test  模块名 测试文件名来创建测试文件

注册服务
--------

.. code-block:: text

   <?php namespace System;

   /**
    * 定义模块名称
    */
   protected $name = 'system';

   // 注册事件监听器
   protected $listens = [
       \Illuminate\Auth\Events\Failed::class               => [
           Listeners\AuthFailed\LogListener::class,
       ],
   ];

   // 注册策略
   protected $policies = [
       PamRole::class    => PamRolePolicy::class,
       PamAccount::class => PamAccountPolicy::class,
   ];

   /**
    * 启动
    */
   public function boot()
   {
       parent::boot($this->name);

       // 注册 api 文档配置
       $this->publishes([
           __DIR__ . '/../resources/config/sami.php'   => storage_path('sami/config.php'),
           __DIR__ . '/../resources/config/module.php' => base_path('config/module.php'),
       ], 'poppy-module');

       $path = poppy_path($this->name);

       // 配置文件
       $this->mergeConfigFrom($path . '/resources/config/module.php', 'module');

       $this->bootConfigMail();
   }

   // 注册服务
   public function register()
   {
       $this->app->register(Http\MiddlewareServiceProvider::class);
       // ...
       $this->app->register(Permission\PermissionServiceProvider::class);

       $this->registerConsole();

       $this->registerSchedule();
   }

   // 计划任务
   private function registerSchedule()
   {
       app('events')->listen('console.schedule', function (Schedule $schedule) {
           $schedule->command('system:user', ['auto_enable'])
               ->everyFiveMinutes()->appendOutputTo($this->consoleLog());
       });
   }

   // 注册命令
   private function registerConsole()
   {
       $this->commands([
           Commands\UserCommand::class,
           ...
           Permission\Commands\PermissionCommand::class,
       ]);
   }

   // 注册 singleton 服务
   private function registerAuth()
   {
       $this->app->singleton('system.form', function ($app) {
           $form = new FormBuilder($app['html'], $app['url'], $app['view'], $app['session.store']->token());

           return $form->setSessionStore($app['session.store']);
       });
   }

   /**
    * 加载扩展
    */
   private function registerAddons()
   {
       (new AddonLoader($this->app))->register();
   }

   // config 覆盖 
   private function bootConfigMail()
   {
       config([
           'mail.driver'       => sys_setting('system::mail.driver') ?: config('mail.driver'),
       ]);
   }

   // 返回服务提供者
   public function provides(): array
   {
       return [
           'system.form',
       ];
   }

验证
----

.. code-block:: text

   1.语言包
    - resource/lang/zh/validator.php
   2.相应的验证规则
    - Rule 类
   3.FrameworkServiceProvider.php 验证正则规则
