Pusher PHP Library
==================

This is a very simple PHP library to the Pusher API (http://pusherapp.com).
Using it is easy as pie:

    require('Pusher.php');

    $pusher = new Pusher($key, $secret, $app_id);
    $pusher->trigger('my-channel', 'my_event', 'hello world');

Debugging
---------
You can either turn on debugging by setting the third argument like so, to true:

    $pusher->trigger('my-channel', 'event', 'data', true)

or with all requests:

    $pusher = new Pusher($key, $secret, $app_id, true);

On failed requests, this will return the server's response, instead of false.

Socket id
---------
In order to avoid duplicates you can optionally specify the sender's socket id while triggering an event (http://pusherapp.com/docs/duplicates):

    $pusher->trigger('my-channel','event','data','socket_id');

Private channels
----------------
To authorise your users to access private channels on Pusher, you can use the socket_auth function:

    $pusher->socket_auth('my-channel','socket_id');

Presence channels
-----------------
Using presence channels is similar to private channels, but you can specify extra data to identify that particular user:

    $pusher->presence_auth('my-channel','socket_id', 'user_id', 'user_info');

Presence example
----------------

First set this variable in your JS app:

    Pusher.auth_url = '/presence_auth.php';

Next, create the following in presence_auth.php:

    <?php
    header('Content-Type: application/json');
    if ($_SESSION['user_id']){
      $sql = "SELECT * FROM `users` WHERE id='$_SESSION[user_id]'";
      $result = mysql_query($sql,$mysql);
      $user = mysql_fetch_assoc($result);
    } else {
      die('aaargh, no-one is logged in')
    }
    
    $pusher = new Pusher($key, $secret, $app_id);
    $presence_data = array('name' => $user['name']);
    echo $pusher->presence_auth($_POST['channel_name'], $_POST['socket_id'], $user['id'], $presence_data);
    ?>

Note: this assumes that you store your users in a table called `users` and that those users have a `name` column. It also assumes that you have a login mechanism that stores the `user_id` of the logged in user in the session.
  

License
-------
Copyright 2010, Squeeks. Licensed under the MIT license: http://www.opensource.org/licenses/mit-license.php 

