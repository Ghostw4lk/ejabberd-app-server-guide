# ejabberd-app-server-guide
Description: This guide shows how to enable push notifications in the app (client side) and how you can notify your users throuh Firebase Cloud Messaging (server side). 

### Requirements:
- Installed Ejabberd 17.09 (cloned from the github repository)
- Smack API (Android based)
- Firebase Cloud Messaging (FCM) enabled in the app

### Step 1: Enable push notification service (client side)
After your user is logged in, you can execute the command to enable the push service with the following code.

```java
String deviceToken = FirebaseInstanceId.getInstance().getToken();
HashMap<String, String> publishOptions = new HashMap<>();
publishOptions.put("secret", deviceToken);

PushNotificationsManager pushNotificationsManager = PushNotificationsManager.getInstanceFor(mConnection);
pushNotificationsManager.enable(pubsub.localhost, pushnode);
```

Explanation of the HashMap object: You can pass publish options to the ejabberd server to identify, which user you have to notify.
In this case the "deviceToken" represents a Firebase token.

Explanation of the "enable"-method:
The first argument is a pushJid, which is by default pubsub.localhost. This JID is also used when you publish items to a pubsub node.
The second argument is a node, this represents a pubsub node.

Thats it on the client side, we have not to do more here.

Source: https://github.com/igniterealtime/Smack/blob/master/documentation/extensions/pushnotifications.md


### Step 2: Modify the mod_push.erl file
We have to modify the mod_push.erl file, because we need the publish options on our app server to know, which user needs to be notified.

Location of the mod_push: YourEjabberdFolder/src/mod_push.erl

Replace line 428 with the following code:<Enter>
``` Item = #ps_item{xml_els = [xmpp:encode(#push_notification{xdata = XData})]},```

Now, go your ejabberd folder and execute the following commands.
1. make               (Compiles the modified mod_push.erl)
2. make install
3. ejabberdctl stop   (If your ejabberd server is currently running).
4. ejabberdctl start
5. ejabberdctl status (Make sure your server is running fine).


### Step 3: App server (server side)

[WORK IN PROGRESS]


### Step 4: Test the app server

[WORK IN PROGRESS]








