.. role:: raw-html-m2r(raw)
   :format: html


菜单(menus)
-----------

支持的类型
^^^^^^^^^^

key 包含 backend(后端), develop(开发), web(前台)这几个部分. 

.. code-block:: text

   backend:
       title: 地区管理
       icon: 'fa fa-location-arrow'
       groups:
           -
               title: 地区管理
               icon: 'fa fa-location-arrow'
               injection : 'system/backend'
               children:
                   -
                       title: 地区管理
                       route: area:backend.content.index
                       match:
                           - order:backend.server.establish
   develop:
       title: 核心开发
       groups:
           -
               title: 文档
               children:
                   -
                       title: Restful-前台
                       route: system:develop.doc.index

key 说明
^^^^^^^^

title : 标题\ :raw-html-m2r:`<br />`
icon : 支持的图标\ :raw-html-m2r:`<br />`
groups : 分组(数组)\ :raw-html-m2r:`<br />`
injection : 分组的插入目标地 'system/backend' 代表放置到system 模块的后台分组\ :raw-html-m2r:`<br />`
match : 用于路由的第三级别, 页面中内容区域和右侧菜单项目的匹配, 不设置可能会导致页面菜单为空
