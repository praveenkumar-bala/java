
# Cold Publisher

A cold publisher is a type of reactive publisher that only starts emitting items when a subscriber subscribes to it. Each subscriber to a cold publisher receives the full sequence of items from the beginning.

The two main types of cold publishers in reactive programming are `Mono` and `Flux`

Cold publishers are suitable for scenarios where you want each subscriber to receive the entire sequence of items independently, and subscribers don't share the emitted items.

```java
import reactor.core.publisher.Flux;

public class ColdPublisherExample {

    public static void main(String[] args) {
        Flux<Integer> coldFlux = Flux.just(1, 2, 3, 4, 5);

        // Subscriber 1
        coldFlux.subscribe(value -> System.out.println("Subscriber 1: " + value));

        // Subscriber 2
        coldFlux.subscribe(value -> System.out.println("Subscriber 2: " + value));
    }
}
```

In this example, both Subscriber 1 and Subscriber 2 receive the full sequence of items (1, 2, 3, 4, 5) independently when they subscribe to the cold Flux.
# Hot Publisher

A hot publisher emits items regardless of whether there are subscribers, and new subscribers may miss previously emitted items. In other words, the sequence of items is independent of the subscribers' presence or absence.

Here are some characteristics of hot publishers:

1. **Emit Items Independently:**
   - Hot publishers emit items continuously, irrespective of whether there are subscribers at any given moment.

2. **No Replay for Late Subscribers:**
   - Subscribers to a hot publisher do not receive previously emitted items. They start receiving items from the point of subscription onward.

3. **Examples of Hot Publishers:**
   - **Subjects:** In reactive libraries like Reactor or RxJava, a `Subject` is a common hot publisher. Examples include `DirectProcessor`, `UnicastProcessor`, or `EmitProcessor`.
   - **Event Buses:** Hot publishers are often used in event-driven architectures, where events or messages are broadcast to multiple subscribers.

4. **Use Cases:**
   - Broadcasting events or messages to multiple components in a system.
   - Implementing a shared state that multiple components can observe.

Here's a simple example using Project Reactor's `DirectProcessor`:

```java
import reactor.core.publisher.DirectProcessor;
import reactor.core.publisher.Flux;

public class HotPublisherExample {

    public static void main(String[] args) {
        // Create a hot publisher using DirectProcessor
        DirectProcessor<String> hotPublisher = DirectProcessor.create();

        // Convert the hot publisher to a Flux for subscribers
        Flux<String> hotFlux = hotPublisher;

        // Subscribe 1
        hotFlux.subscribe(value -> System.out.println("Subscriber 1: " + value));

        // Emit an item
        hotPublisher.onNext("Item 1");

        // Subscribe 2
        hotFlux.subscribe(value -> System.out.println("Subscriber 2: " + value));

        // Emit another item
        hotPublisher.onNext("Item 2");
    }
}
```

In this example, both Subscriber 1 and Subscriber 2 receive items emitted by the hot publisher (`DirectProcessor`), and items are emitted regardless of whether there are subscribers. Hot publishers are useful in scenarios where you want to broadcast events or share a stream of data among multiple components.


