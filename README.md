# EventHub

EventHub is a observer pattern implementation. You can register for events and emit those events.

## Usage
#### Simple events:
```
let hub = EventHub<String, Void>()
_ = hub.on("myEvent") {
	print("Event was fired!")
}
hub.emit("myEvent")
```

#### Sending payload:
```
let hub = EventHub<String, Int>()
_ = hub.on("increment") { count in
	print("New count is \(count)")
}
hub.emit("increment", with: 4)
```

#### Only fire once and unregisters:
```
hub.once("increment") { count in
	...
}
```

#### Manual dispose event registration:
```
let dis = hub.on("myEvent") { ... }
...
dis.dispose()
```

#### Using dispose bag:
Sometimes you want a registration to be alive as long as a certain object is still in memory.
```
let bag = DisposeBag()
hub.on("myEvent") { ... }.addTo(bag)
```

#### Fire events on specific queue:
With a specific queue dispatching the event is always async.
```
hub.emit("myEvent", on: DispatchQueue.main, with: 50)
```

#### Remove listener from the hub
```
hub.removeAllListeners(forEvent: "myEvent") //only for the given event
hub.removeAllListeners()										//all listeners get removed
```

#### Payload evaluation is lazy
```
//function is only evaluated if there are listeners registered
hub.emit("myEvent", with: expensiveToCalculateObject())
```

## Installation

Include EventHub via CocoaPods