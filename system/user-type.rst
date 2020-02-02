
用户类型/角色/加密方式
----------------------

本系统基于角色的权限访问控制（Role-Based Access Control）

用户类型
^^^^^^^^

根据使用分为三种用户类型(默认)

.. code-block:: text

   user            # 前台用户
   develop         # 开发者用户
   backend         # 后台用户

根据使用到的用户的类型我们应当分为这几项

.. code-block:: text

   用户 - api (jwt) 驱动
   用户 - web 驱动
   开发 - web 驱动
   后端 - web 驱动
   后端 - api (jwt) 驱动

每一种用户定义的角色分为三种

.. code-block:: text

   root     : 后台超级管理员
   user     : 前台普通用户
   develop  : 开发者

密码加密方式
^^^^^^^^^^^^

.. code-block:: text

   $password     : 原始密码
   $reg_datetime : 注册时间(datetime) 类型
   $randomKey    : 六位随机值
   md5(sha1($password . $reg_datetime) . $randomKey);
