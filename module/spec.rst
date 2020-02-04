
模块约定
------------

缓存
^^^^

缓存 Key 命名
~~~~~~~~~~~~~~

缓存Key 放置在模型/Action 的常量中, 命名规范是

``CK_{Key}``

Key : 命名的标识

缓存命中命名
~~~~~~~~~~~~~

缓存命中包含 Atomic / Funnel 这种, 缓存命中放置在 ``Redis(0)`` 仓库中, 命名规范为

``prefix:redis:{function tag}:{key}``

function tag : 功能标签, 用来进行文件夹区分
key          : 唯一标识符

