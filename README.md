CocoaMQTT
=========

MQTT v3.1.1 client library for iOS and OS X written with Swift 2


Build
=====

Build with Xcode 7.0 / Swift 2


Installation
=====
###CocoaPods
Install using [CocoaPods](http://cocoapods.org) by adding this line to your Podfile:

````ruby
use_frameworks! # Add this if you are targeting iOS 8+ or using Swift
pod 'CocoaMQTT'  
````
Then, run the following command:

```bash
$ pod install
```

###Carthage
Install using [Carthage](https://github.com/Carthage/Carthage) by adding this line to your Cartfile:

````
github "emqtt/CocoaMQTT"
````
Then, run the following command:

```bash
$ carthage update
```
Last if you're building for OS X:

- On your application targets “General” settings tab, in the “Embedded Binaries” section, drag and drop CocoaMQTT.framework from the Carthage/Build/Mac folder on disk.

If you're building for iOS, tvOS:

- On your application targets “General” settings tab, in the “Linked Frameworks and Libraries” section, drag and drop each framework you want to use from the Carthage/Build folder on disk.

- On your application targets “Build Phases” settings tab, click the “+” icon and choose “New Run Script Phase”. Create a Run Script with the following contents: 
`/usr/local/bin/carthage copy-frameworks`

- and add the paths to the frameworks you want to use under “Input Files”, e.g.:
```
$(SRCROOT)/Carthage/Build/iOS/CocoaMQTT.framework
```

#### Manual
- Open up Terminal, `cd` into your top-level project directory, and run the following command "if" your project is not initialized as a git repository:

```bash
$ git init
```

- Add CocoaMQTT as a git [submodule](http://git-scm.com/docs/git-submodule) by running the following command:

```bash
$ git submodule add https://github.com/emqtt/CocoaMQTT.git
```

- Open the new `CocoaMQTT` folder, and drag the `CocoaMQTT.xcodeproj` into the Project Navigator of your application's Xcode project.

- Next, select your application project in the Project Navigator (blue project icon) to navigate to the target configuration window and select the application target under the "Targets" heading in the sidebar.

- In the tab bar at the top of that window, open the "General" panel. Click on the + button under the "Embedded Binaries" section.
    
- Then select the CocoaMQTT.framework inside `CocoaMQTT.xcodeproj` (to choose iOS or OSX according to your need)



Usage
=====

Example in Example project:

```swift
let mqttCli = CocoaMQTTCli()
let clientIdPid = "CocoaMQTT-" + String(NSProcessInfo().processIdentifier)
let mqtt = CocoaMQTT(clientId: clientIdPid, host: "localhost", port: 1883)
mqtt.username = "test"
mqtt.password = "public"
mqtt.willMessage = CocoaMQTTWill(topic: "/will", message: "dieout")
mqtt.keepAlive = 90
mqtt.delegate = mqttCli
mqtt.connect()

dispatch_main()
```


CocoaMQTT
==========

```swift
/**
 * Blueprint of the mqtt client
 **/
protocol CocoaMQTTClient {
    
    var host: String { get set }
    
    var port: UInt16 { get set }
    
    var clientId: String { get }
    
    var username: String? {get set}
    
    var password: String? {get set}
    
    var cleansess: Bool {get set}
    
    var keepAlive: UInt16 {get set}
    
    var willMessage: CocoaMQTTWill? {get set}
    
    func connect() -> Bool
    
    func publish(topic: String, withString string: String, qos: CocoaMQTTQOS) -> UInt16
    
    func publish(message: CocoaMQTTMessage) -> UInt16
    
    func subscribe(topic: String, qos: CocoaMQTTQOS) -> UInt16
    
    func unsubscribe(topic: String) -> UInt16
    
    func ping()
    
    func disconnect()
    
}
```


CocoaMQTTDelegate
=================

```swift
protocol CocoaMQTTDelegate {
    
    /**
     * MQTT connected with server
     */
    func mqtt(mqtt: CocoaMQTT, didConnect host: String, port: Int)
    
    func mqtt(mqtt: CocoaMQTT, didConnectAck ack: CocoaMQTTConnAck)
    
    func mqtt(mqtt: CocoaMQTT, didPublishMessage message: CocoaMQTTMessage, id: UInt16)
    
    func mqtt(mqtt: CocoaMQTT, didReceiveMessage message: CocoaMQTTMessage, id: UInt16 )
    
    func mqtt(mqtt: CocoaMQTT, didSubscribeTopic topic: String)
    
    func mqtt(mqtt: CocoaMQTT, didUnsubscribeTopic topic: String)
    
    func mqttDidPing(mqtt: CocoaMQTT)
    
    func mqttDidReceivePong(mqtt: CocoaMQTT)
    
    func mqttDidDisconnect(mqtt: CocoaMQTT, withError err: NSError)

}
```


AsyncSocket and Timer
=====================

These third-party functions are used:

* [GCDAsyncSocket.h](https://github.com/robbiehanson/CocoaAsyncSocket)
* [MSWeakTimer.h](https://github.com/mindsnacks/MSWeakTimer)


LICENSE
=======

MIT License (see `LICENSE`)

## Contributors

* [@andypiper](https://github.com/andypiper)
* [@turtleDeng](https://github.com/turtleDeng)


Author
======

Feng Lee <feng@emqtt.io>


Twitter
======

https://twitter.com/emqtt


