Web Socket Chat
===============
Online chat based on web sockets and ratchet php

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist joni-jones/yii2-wschat "*"
```

or add

```
"joni-jones/yii2-wschat": "*"
```

to the require section of your `composer.json` file.

Usage
------------

1. To start chat server need to create console command and setup it as demon:
    
    - Create controller which extends `yii\console\controller`:
        
        ```php
        ServerController extends \yii\console\controller
        ```
        
    - Create action to start server:
    
        ```php
        public function actionRun()
        {
            $server = IoServer::factory(new HttpServer(new WsServer(new Chat(new ChatManager()))), 8080);
            $server->run();
        }
        ```
        
    If you want to use chat for auth users, you must to specify `userClassName` property for `ChatManager` instance.
    For example:
    
        ```php
        $manager = Yii::configure(new ChatManager(), [
            'userClassName' => '\yii\db\ActiveRecord' //allow to get users from MySQL or PostgreSQL
        ]);
        ```
        
    - Now, you can run chat server with `yii` console command:
    
        ```php
        yii server/run
        ```
        
2. To add chat on page just call:

    ```php
    <?=ChatWidget::widget();?>
    ```
    
    or if you want to use chat for auth users just add config as parameter:
      
        <?=ChatWidget::widget([
            'auth' => true,
            'user_id' => '' // setup id of current logged user
        ]);?>
    
    
        List of available options:
            auth - boolean, default: false
            user_id - mixed, default: null
            port - integer, default: 8080
            chatList - array (allow to set list of preloaded chats), default: [
                id => 1,
                title => 'All'
            ]

You can also store added chat, just specify js callback for vent events:

    Chat.vent('chat:add', function(chatModel) {
        console.log(chatModel);
    });
    
In the callback you will get access to ``Chat.Models.ChatRoom`` backbone model.

License
----

MIT