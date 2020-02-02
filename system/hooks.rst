
钩子(hook)
----------

服务的位置: ``modules/{module}/configurations/services.yaml``

hook 位置: ``modules/{module}/configurations/hooks.yaml``

服务和钩子的概念
^^^^^^^^^^^^^^^^

我们来解释一下service：


#. service是 module 之间扩展的重要方式。
#. 我们将 service 和 hook 看作是插槽与插头的关系。一个插槽可以插多个插头。
#. 每个app下都会有一个service.yaml的文件，来描述本app的service,。

使用
^^^^^^^^^^^^^^^^

Array 类型
~~~~~~~~~~~
**定义service**

首先再services.yaml中定义如下内容

.. code-block:: yaml

   system.api_info:
      title: 系统接口
      type: array
      description: 系统信息接口调用, 系统信息返回的灵活数据

**定义 hooks**
然后再hooks.yaml文件中,注册调用hook方法

.. code-block:: yaml

   -
       name: 'system.api_info'
       hooks:
           - '\System\Services\Hooks\ApiInfo'

编写实现对应的key()/data()方法

.. code-block:: php

   <?php
   class ApiInfo
       public function key()
       {
           return 'api';
       }

       public function data()
       {
           return 'info';
       }
   }

执行 ServiceFactory的parse方法

.. code-block:: php

   sys_hook('system.api_info')
   [
       'api' => 'info'
   ]

Form
~~~~

定义service, 这个 Service是单选
首先再services.yaml中定义如下内容

.. code-block:: yaml

   ad.place_selection:
       title: 广告位选择
       type: form
       description: 选择广告位

注册hook方法

.. code-block:: yaml

   -
       name: 'ad.place_selection'
       builder: '\Ad\Services\Hooks\AdPlaceSelection'

实现builder方法

.. code-block:: php

   public function builder($params = [])
   {
      $name    = $params['name'];
      $value   = $params['value'] ?? null;
      $options = $params['options'] ?? [];

      $options  += [
         'class'       => 'layui-input',
         'placeholder' => '请选择广告位',
      ];
      $places = AdPlace::pluck('title', 'id');
      return \Form::select($name, $places, $value, $options);
   }

调用执行

.. code-block:: php

   sys_hook('ad.place_selection', $param)

代码实现
^^^^^^^^

.. code-block:: php

   <?php
       public function parse($id, $params = [])
       {
           $service = app('module')->services()->get($id);
           if (!$service) {
               return null;
           }
           $hooks = app('module')->hooks()->get($id);

           $method = 'parse' . studly_case($service['type']);

           if (\is_callable([$this, $method])) {
               return $this->$method($hooks, $params);
           }
           return null;
       }

获取service
~~~~~~~~~~~

services方法中调用 ModulesService发initialize方法中,对每个模块下的service的配置进行了key=>value的缓存初始化操作

.. code-block:: php

       /**
        * @return ModulesService(
        */
       public function services(): ModulesService
       {
           if (!$this->serviceRepo instanceof ModulesService) {
               $collect = collect();
               $this->repository()->enabled()->each(function (Module $module) use ($collect) {
                   $collect->put($module->slug(), $module->get('services', []));
               });
               $this->serviceRepo = new ModulesService();
               $this->serviceRepo->initialize($collect);
           }

           return $this->serviceRepo;
       }

.. code-block:: php

       /**
        * Initialize.
        * @param Collection $data 集合
        */
       public function initialize(Collection $data)
       {
           $this->items = $this->getCache('poppy')->remember(
               'modules.service', SysConfig::MIN_DEBUG,
               function () use ($data) {
                   $collection = collect();
                   $data->each(function ($items) use ($collection) {
                       $items = collect($items);
                       $items->each(function ($item, $key) use ($collection) {
                           $collection->put($key, $item);
                       });
                   });
                   return $collection->all();
               }
           );
       }

然后通过get()方法获取指定key的相关service配置

.. code-block:: php

       /**
        * Get a module by name.
        * @param mixed $name name
        * @return Module
        */
       public function get($name): Module
       {
           return $this->repository()->get($name);
       }

.. code-block:: php

       /**
        * @return Modules
        */
       public function repository(): Modules
       {
           if (!$this->repository instanceof Modules) {
               $this->repository = new Modules();
               $slugs            = app('poppy')->enabled()->pluck('slug');
               $this->repository->initialize($slugs);
           }

           return $this->repository;
       }

获取注册的hook方法
~~~~~~~~~~~~~~~~~~

.. code-block:: php

   $hooks = app('module')->hooks()->get($id);

.. code-block:: php

       /**
        * @return ModulesHook
        */
       public function hooks(): ModulesHook
       {
           if (!$this->hooksRepo instanceof ModulesHook) {
               $collect = collect();
               $this->repository()->enabled()->each(function (Module $module) use ($collect) {
                   $collect->put($module->slug(), $module->get('hooks', []));
               });
               $this->hooksRepo = new ModulesHook();
               $this->hooksRepo->initialize($collect);
           }

           return $this->hooksRepo;
       }

.. code-block:: php

       /**
        * Initialize.
        * @param Collection $data 集合
        */
       public function initialize(Collection $data)
       {
           $this->items = $this->getCache('poppy')->remember(
               'modules.hooks', SysConfig::MIN_DEBUG,
               function () use ($data) {
                   $collection = collect();
                   $data->each(function ($items) use ($collection) {
                       $items = collect($items);
                       $items->each(function ($item) use ($collection) {
                           $data    = (array) $collection->get($item['name']);
                           $service = app('module')->services()->get($item['name']);
                           if ($service['type'] === 'array') {
                               $collection->put($item['name'], $data + $item['hooks']);
                           }
                           if ($service['type'] === 'form') {
                               $collection->put($item['name'], $item['builder']);
                           }
                       });
                   });
                   return $collection->all();
               }
           );
       }

执行相应的parseArray /parseForm 方法
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

   $method = 'parse' . studly_case($service['type']);

   if (\is_callable([$this, $method])) {
       return $this->$method($hooks, $params);
   }

.. code-block:: php

       private function parseArray($hooks, $params)
       {
           $collect = [];
           collect($hooks)->each(function ($hook) use (&$collect) {
               if (class_exists($hook)) {
                   $obj = new $hook();
                   if ($obj instanceof ServiceArray) {
                       $collect = array_merge($collect, [
                           $obj->key() => $obj->data(),
                       ]);
                   }
               }
           });
           return $collect;
       }

.. code-block:: php

       private function parseForm($builder, $params)
       {
           if (class_exists($builder)) {
               $obj = new $builder();
               if ($obj instanceof ServiceForm) {
                   return $obj->builder($params);
               }
           }
           return '';
       }

调用hook定义的对应的方法

.. code-block:: php

       public function key()
       {
           return 'api';
       }

       public function data()
       {
           return 'info';
       }

.. code-block:: php

       public function builder($params = [])
       {
           $name    = $params['name'];
           $value   = $params['value'] ?? null;
           $options = $params['options'] ?? [];

           $options  += [
               'class'       => 'layui-input',
               'placeholder' => '请选择广告位',
           ];
           $places = AdPlace::pluck('title', 'id');
           return \Form::select($name, $places, $value, $options);
       }

运行结果
~~~~~~~~

.. code-block:: php

   dump((new ServiceFactory)->parse('system.api_info'));

   /**
    *  array:2 [
    *      "api" => "info"
    *      "api2" => "info2"
    *  ]
    *
    */


   dump((new ServiceFactory)->parse('ad.place_selection', [
       'name' => 'abc'
   ]));
   // Illuminate\Support\HtmlString {#619
     #html: "<select class="layui-input" name="abc"><option selected="selected" value="">请选择广告位</option><option value="4">东城区</option><option value="5">北京市</option><option value="7">轮播图</option></select>"
   }
