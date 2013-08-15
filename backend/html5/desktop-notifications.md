html5 桌面通知实现

参考实现：http://www.html5rocks.com/en/tutorials/notifications/quick/

###实现步骤

1. 检查notification api 是否支持

    ``` javascript
    // check for notifications support
// you can omit the 'window' keyword
if (window.webkitNotifications) {
  console.log("Notifications are supported!");
}
else {
  console.log("Notifications are not supported for this Browser/OS version yet.");
}

    ```

2. 让用户授权网站可以弹出通知

    ```javascript
    
    document.querySelector('#show_button').addEventListener('click', function() {
      if (window.webkitNotifications.checkPermission() == 0) { // 0 is PERMISSION_ALLOWED
        // function defined in step 2
        window.webkitNotifications.createNotification(
            'icon.png', 'Notification Title', 'Notification content...');
      } else {
        window.webkitNotifications.requestPermission();
      }
    }, false);
    
    ```
3. Attach listeners and other actions 

    ```javascript
    
    document.querySelector('#show_button').addEventListener('click', function() {
  if (window.webkitNotifications.checkPermission() == 0) { // 0 is PERMISSION_ALLOWED
    // function defined in step 2
    notification_test = window.webkitNotifications.createNotification(
      'icon.png', 'Notification Title', 'Notification content...');
    notification_test.ondisplay = function() { ... do something ... };
    notification_test.onclose = function() { ... do something else ... };
    notification_test.show();
  } else {
    window.webkitNotifications.requestPermission();
  }
}, false);
    
    ```    

###插件实现

* https://github.com/azproduction/jquery.notification


