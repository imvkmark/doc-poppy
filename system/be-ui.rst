
后台 UI 标准
------------

页面规范
^^^^^^^^^^^

**页面采用 Card 嵌套**

.. code-block:: html

   <div class="layui-card-header">
       用户列表
   </div>
   <div class="layui-card-body">
   </div>

**页面右上角按钮采用 Normal 样式区分, 右浮动**

.. code-block:: html

   <div class="pull-right">
       <a href="#" class="layui-btn layui-btn-normal">新增</a>
   </div>

**普通的提交采用默认样式**

.. code-block:: html

   <button type="submit" class="layui-btn">
       <i class="fa fa-search"></i> 搜索
   </button>

按钮
^^^^^^^

**普通图标需要有 tooltip**


* 需要有 ``J_tooltip``
* 需要有 ``title``

.. code-block:: html

   <a href="#"
       class="J_request J_tooltip" data-confirm="确认删除 {{ $item->title }} ?" title="删除">
       <i class="fa fa-times text-danger"></i>
   </a>

**文字按钮使用边框按钮(根据)**

.. code-block:: html

   <a href="#" class="layui-btn layui-btn-primary layui-btn-xs">
       修改权限
   </a>

分页
^^^^^^^

分页不需要加任何的包裹

.. code-block:: html

   {!! $items->render() !!}

弹窗
^^^^^^^

弹出窗口使用 J_dialog 加载, 需要继承 dialog 模版

.. code-block:: html

   @extends('system::backend.tpl.dialog')
   @section('backend-main')
       // This is Content
   @endsection
