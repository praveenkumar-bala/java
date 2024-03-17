
In Traditional Application, If we send a request, the web server will create Thread to each request and it will wait for the request to complete some IO operations. Like writing a database.
That means it is synchronous and blocking. 

Think about it in Microservices Application, Multiple services can communicate each other. if it is synchronous and blocking then you will see the huge delay in the response.




Reactive programing will help us to achieve
1. Asynchronous
2. Non blocking
3. Event Driven

How this is achieved?


#### Reactive Streams Specification - https://www.reactive-streams.org

Reactive Streams is an initiative to provide a standard for asynchronous stream processing with non-blocking back pressure. This encompasses efforts aimed at runtime environments (JVM and JavaScript) as well as network protocols.



#### Reactor Programming is based on the Observer Pattern

The Observer Design Pattern is a [behavioral design pattern](https://www.geeksforgeeks.org/behavioral-design-patterns/) that defines a one-to-many dependency between objects so that when one object (the subject) changes state, all its dependents (observers) are notified and updated automatically.

Reactor Streams Specification defines few important interfaces

1. Publisher
2. Subscriber
3. Subscription
4. Processor

#### Publisher
Publisher. publishes the messages to the subscriber.

#### Subscriber (Receiver of the message)
Subscriber follows the publisher 
#### Subscription
Communication between publisher and subscriber will happen through the subscription object. 

Subscriber will use Subscription object to request or cancel the messages

![[Pasted image 20240224100833.png]]


#### Processor
It will be acting both subscriber and producer.



# Publisher / Subscriber Communication

1. Publisher. - Producer of an items received by subscribers
2. Subscriber - Receivers on an Items


```

@FunctionalInterface  
public static interface Publisher<T> {  
	public void subscribe(Subscriber<? super T> subscriber);  
}

@FunctionalInterface  
public static interface Subscriber<T> {  
    public void onSubscribe(Subscription subscription);  
    public void onNext(T item);  
    public void onError(Throwable throwable);  
	public void onComplete();  
}

@FunctionalInterface
public static interface Subscription {  
	public void request(long n);  
	public void cancel();  
}

```

Here we are going to talking about how publisher / subscriber communication happens.

1. Subscriber calls **Producer.subscribe** method to initiate the communication
2. If Publisher accepts the subscription then Publisher will call **Subscriber.onSubscribe** with **subscription** object
3. Then Subscriber can use the Subscription object to request n items to the Publisher using **Subscription.request(n)** or it can use **cancel()** method to cancel the subscriptions.
4. If Publisher send all the message, and it doesn't have nothing to send then they will use **Subscriber.onComplete()** method to notify the Subscriber it is done.
5. if Publisher faces error then it will use **Subscriber.onError** method to notify error to the Subscriber


# Reactor

Reactive stream is the specification and Reactor is the implementation of that specification.

Project Reactor provides two different implementations of Publisher
1. **Mono**
2. **Flux**
#### Mono

- It emits 0 or 1 Items followed by an onComplete or onError
#### Flux
- It emits 0 or N items followed by onComplete or onError


### Ways of creating Mono

1. If you have predefined data or you have data already then use `Mono.just`

```

Mono<Integer> mono = Mono.just(1);

mono.subscribe(i -> System.out.println("Received : " + i));

```

2. if you going to calculate anything new based on subscriber then use Supplier
```
Supplier<String> stringSupplier = () -> Util.faker().name().fullName();  
mono = Mono.fromSupplier(stringSupplier);  
mono.subscribe(i -> System.out.println("Received : " + i));
```
3. `Mono.fromCallable` - using `Callable<T>` to generate Mono
4. `Mono.fromFuture` - Using `CompletableFuture<T>`to generate Mono
5. `Mono.fromRunnable` - Using `Runnable` to generate Mono
6. `Mono.empty()` - Creating an empty Mono
There are so many ways, we will add new ways at the time of learning

### Ways of creating Flux

```
// Using Just

Flux.just(1,2,3,4,"Name")

// Using Array

Flux.fromArray({ 2, 5, 7, 8})

// Using List or Iterable

 Flux.fromIterable(Arrays.asList("a", "b", "c")).subscribe(Util.onNext());

//Using Stream API
Flux.fromStream(() -> List.of(1, 2, 3, 4, 5).stream());

// Flux from intervel
Flux.range(3, 10)

//Empty
Flux.empty()

//Exception
Flux.error(Throwable)





```


# Flux.create()

Programmatically create a Flux with the capability of emitting multiple elements in a synchronous or asynchronous manner through the FluxSink API. This includes emitting elements from multiple threads

```
Flux.create(fluxSink -> {  
    String country;  
    do{  
        country = Util.faker().country().name();  
        fluxSink.next(country);  
    }while (!country.toLowerCase().equals("canada"));  
    fluxSink.complete();  
})  
.subscribe(Util.subscriber());

```

### FluxSink
- is thread safe way to emitting the data.
- create accept fluxSink consumer and we can use the fluxSink to emit the data

# Take Operator

Take only the first N values from this Flux, if available.

If N is zero, the resulting Flux completes as soon as this Flux signals its first value (which is not not relayed, though).
 
Note that this operator doesn't manipulate the backpressure requested amount. Rather, it merely lets requests from downstream propagate as is and cancels once N elements have been emitted. As a result, the source could produce a lot of extraneous elements in the meantime. If that behavior is undesirable and you do not own the request from downstream (e.g. prefetching operators), consider using limitRequest(long) instead.

The problem with `Flux.create` is, let consider create will generate 10 items and subscriber wants to access only 3 then after the 3 item consumer will send cancel signal, how ever producer will not stop to produce the element if we are not handled properly.

# Generate

1. Generate is an alternative for create
2. Generate will accept state parameter and SynchronousSink
3. SynchronousSink will emit only one item along with complete or cancel signals
4. Generate will taken care of looping and other things

```
// exit if contry name is canada  
// exit if max = 10  
// exit subscriber cancels - exit  
  
Flux.generate(  
                () -> 1,  
                (counter, sink) -> {  
                    String country = Util.faker().country().name();  
                    sink.next(country);  
                    if (counter >= 10 || country.toLowerCase().equals("canada"))  
                        sink.complete();  
                    return counter + 1;  
                }  
        )  
        .take(4)  
        .subscribe((countryName) -> {  
            System.out.println(" The Country name is " + countryName);  
        });

```


### SynchronousSink

it will emit only one item at a time. while fluxSink will emit n number of item at a time.

Meaning 
`Flux.generate` - is responsible to do loop, 

For example, when you use generate to create a publisher. generate method will handle the looping based on subscriber request. 

Note:
You cannot emit multiple items at a time.



# Flux.push

push is not thread safe. and this is for single thread producer.



# Operator

