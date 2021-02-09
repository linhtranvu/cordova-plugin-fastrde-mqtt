# cordova-plugin-fastrde-mqtt
This is a fork from **cordova-plugin-fastrde-mqtt** (https://github.com/fastrde/cordova-plugin-fastrde-mqtt  with [some fix from IOCare)](https://github.com/IOCare/cordova-plugin-fastrde-mqtt), a good plugin for MQTT known working for both IOS and Android. I fix some dependency to make it works again and update Documentation

## Successfully testing

Now working smoothly on both **IOS** and **Android**. Tested on Jan 2021, Cordova 9. XCode 12.3

## Installation Android

`cordova plugin add https://github.com/linhtranvu/cordova-plugin-fastrde-mqtt.git`

## Installation IOS

#### Fresh Install

First, you need to run install command directly on IOS

`cordova plugin add https://github.com/linhtranvu/cordova-plugin-fastrde-mqtt.git`

Install from `package.json` (mean you install on Android, and copy app to MacOS) will cause missing dependency.

#### Add and remove IOS Platform

Sometime, we need to add, remove IOS platform and add again. In this case, you need to uninstall this package first

`cordova plugin remove cordova-plugin-fastrde-mqtt ` 

Now you can safely remove IOS platform and add again, then reinstall plugin as fresh

> Sorry for this buggy installation thing, but at least we have a working MQTT plugin 

## API

#### mqtt.init(options)

Initialize the mqtt-client with the given options.
```
    host - borker to connect to [required]
    port - port to connect to [default 1883]
    qos - Quality of Service level[default 0]
    clientId - Unique Identifier for the Client [default cordova_mqtt_<random>]
    username - username for broker authentication [default none] 
    password - password for broker authentication [default none]
    ssl - should ssl be used [default false]
    keepAlive - keepAlive sending interval in seconds [default 10]
    timeout - session timeouts after <timeout> seconds [default 30]
    cleanSession - clean Session at disconnect [default true]
    protocol - mqtt protocol level [default 4] 
    offlineCaching - NOT TESTED, so you should change to false! Should mesages be cached in sqlite before sending [default true].
```
#### mqtt.connect()
connect to the broker with the initial given options.
#### mqtt.disconnect()
disconnect from the broker.
#### mqtt.publish(message)
Send a Message.
```
    topic - topic where the message is send to
    message - payload of the message
    qos - Quality of Service level
    retain - should the message retain in the channel
```
#### mqtt.subscribe(options)
Subscribes to the topic with the given options
```
    topic - topic to subscribe
    qos - Quality of Service of the Subscription
```
#### mqtt.unsubscribe(options)
Subscribes to the topic with the given options
```
    topic - topic to unsubscribe
```
#### mqtt.on(event, success, error)
set callback functions for the given event.
```
    event - could be 
      "init":

      "connect": 
        success(status)
        error(errorMessage)
      "disconnect": 
        success(status)
        error(errorMessage)
      "publish": 
        success(message)
        error(errorMessage)
      "subscribe": 
        success(subscribtion)
        error(errorMessage)
      "unsubscribe": 
        success(topic)
        error(errorMessage)
     "message": 
        success(message)
    
    success - callback that get called on success of the event
    error - callback that get called on error of the event
```
#### mqtt.will(message)
```
    topic - topic where the last will is send to on disconnect
    message - payload of the message
    qos - Quality of Service level
    retain - should the message retain in the channel
```

## Example

### Make connection

```javascript
      mqtt.init({
        host: '192.168.1.11', // - borker to connect to [required]: 
        port: 1883, // - port to connect to [default 1883]
        // qos - Quality of Service level[default 0]
        // clientId - Unique Identifier for the Client [default cordova_mqtt_<random>]
        username: 'username', // - username for broker authentication [default none]
        password: 'password', // - password for broker authentication [default none]
        ssl: false // - should ssl be used [default false]
        keepAlive: 60, // - keepAlive sending interval in seconds [default 10]
        timeout: 30, // - session timeouts after <timeout> seconds [default 30]
        // cleanSession - clean Session at disconnect [default true]
        // protocol - mqtt protocol level [default 4]
        offlineCaching: false // NOT TESTED, so should be false - should mesages be cached in sqlite before sending [default true]

      })
      mqtt.connect()
```

### Pub and Sub

#### Publish

```javascript
      mqtt.publish({
        topic: 'my topic', // - topic where the message is send to
        message: 'my message', // - payload of the message
        qos: 0, // - Quality of Service level
        retain: false // - should the message retain in the channel
      })
```

#### Subscribe

```javascript
     mqtt.subscribe({
        topic: this.sub_topic, // - topic to subscribe
        qos: 0 // - Quality of Service of the Subscription
      })
```

### Get new messages

```javascript
    mqtt.on('message',
      function (s) {
        console.log(s)
        const d = new Date()
        const timeStamp = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds()
        console.log(timeStamp + ' :' + s.topic + ' : ' + s.message)
      })
```

