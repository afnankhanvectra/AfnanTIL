**RX** is use for **MVVM**  pattern. In this , you observe events and change view according to model.

In computing, reactive programming is a programming paradigm oriented around data flows and the propagation of change. This means that it should be possible to express static or dynamic data flows with ease in the programming languages used, and that the underlying execution model will automatically propagate changes through the data flow. — Wikipedia

###Observable:

 everything in RXSwift is observable. We observe all objects that implement observable-sequence and change our view if any event happens.
Like
let helloSequence = Observable.of("Hello Rx")
let subscription = helloSequence.subscribe { event in
  print(event)
}

We are observing hellowSequqnce. If any thing changes than it will be observed.
Observable has 3 events

* 	.next(value: T) — When a value or collection of values is added to an observable sequence it will send the next event to its subscribers as seen above. The associated value will contain the actual value from the sequence.
* 	.error(error: Error) — If an Error is encountered, a sequence will emit an error event. This will also terminate the sequence.
* 	.completed — If a sequence ends normally it sends a completed event to its subscribers




         let helloSequence = Observable.from(["H","e","l","l","o"])
        let subscription = helloSequence.subscribe { event in
         switch event {
         case .next(let value):
          print(value)
         case .error(let error):
          print(error)
         case .completed:
          print("completed")
          }
        }

4th event is most important that is ***“Dispose”*** that handles memory management of observable.
Disposables.create()
Dispose bag can also cancel event

// Creating a DisposeBag so subscription will be cancelled correctly
let bag = DisposeBag()

// Adding the Subscription to a Dispose Bag
subscription.addDisposableTo(bag)

We can assign thread to observe and subscribe. ***MainScheduler()*** is used for main thread and  **“ConcurrentDispatchQueueScheduler(qos: .background)”** . For background thread.Most of the time we observe on mainThread and subscribe on background thread.

It’s easy to create dependencies In RX by using transformation. There are many transformation like map , FlatMap, Merge, Filter, Recude and other. .I use flatmap for that because it passes in other Obsrvable.. One RX result will filter in other RX to pass.

     function1( )
            .flatMap({ (result1) -> Observable<Bool> in
                return function2(receivedText:result1)
                    .observeOn(MainScheduler())
                    .subscribeOn(MainScheduler())
            })
            .observeOn(MainScheduler())
            .subscribeOn(bgSchedular)
            .subscribe(onNext: { (user) in
                print("\(#function) : onNext()")
            }, onError: { (err) in
                print("\(#function) : error: \(err)")
                completion(false , err)
            }, onCompleted: {
                print("\(#function) : onCompleted()")
                completion(true , nil)
            } )


In above statement function1 will call first and it will get result of  (result ) and by using flattop, passing to second function. Its relic will return in onComplete.
