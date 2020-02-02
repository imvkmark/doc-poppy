.. role:: raw-html-m2r(raw)
   :format: html


事件(Event)
-----------

文件位置及命名
^^^^^^^^^^^^^^

事件放置在 ``modules/{module}/events``  文件夹中, 监听器放在 ``modules/{module}/listeners``  文件夹中

事件(Event)
^^^^^^^^^^^


#. 
   事件命名是 DoWhatEvent, 使用 Event 后缀

#. 
   数据的修饰符为 public ,无需定义 get 方法

#. 
   事件需要继承 Poppy\Framework\Application\Event

.. code-block::plain

   <?php namespace Poppy\Framework\Events;

   use Poppy\Framework\Application\Event;

   class LocaleChanged extends Event
   {
       /**
        * @var string
        */
       public $locale;


       public function __construct($locale)
       {
           $this->locale = $locale;
       }
   }

监听(Listener)
^^^^^^^^^^^^^^

监听器放置位置在 ``modules/{module}/listeners/{event_folder}``  这个文件夹下, 文件夹名称和事件的名称相符合, 但是是蛇形写法.\ :raw-html-m2r:`<br />`\ 事件监听器必须为 DoWhatListener, 事件中需要体现 监听器的作用,并且必须是 Listener 后缀

.. code-block::plain

   <?php namespace Order\Listeners\OrderBossIngCancel;

   use Order\Events\OrderBossIngCancelEvent;
   use Order\Models\OrderHunter;
   use Poppy\Extension\NetEase\Im\Yunxin;
   use System\Classes\Traits\ListenerTrait;
   use User\Models\UserProfile;

   /**
    * 订单IM - 老板取消订单
    */
   class BossImListener
   {
       use ListenerTrait;

       /**
        * @param OrderBossIngCancelEvent $event
        */
       public function handle(OrderBossIngCancelEvent $event)
       {
           /** @var OrderHunter $order */
           $order  = $event->order;
           $accid  = UserProfile::nickPic($order->account_id)['accid'];
           $yun    = new Yunxin();
           $Msg    = [
               // ...
           ];
           $result = $yun->sendMsg($Msg);

           $this->listenIm($event, self::class, $result);
       }
   }

事件监听定义
^^^^^^^^^^^^

事件监听放在 ``{module}/src/ServiceProvider.php`` 文件中, 如下定义

.. code-block:: php

   protected $listens = [
       // 这里使用 子命名空间 来引入
       // 这里不使用 字符串 命名
       Event\LoginSuccessEvent::class               => [
           Listeners\LoginSuccess\LogListener::class,
       ],

       // 系统级别采用全命名空间引入
       \Poppy\Framework\Events\LocaleChanged::class => [
           //...
       ]
   ]
