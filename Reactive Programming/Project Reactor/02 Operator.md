
Operators refers to predefined function or method that performs a specific operation on reactive streams or publishers

Operators allow you to manipulate, transform, filter, combine, or otherwise process data within the reactive streams in a declarative and composable manner. 

Here are some common types of operators used in reactive programming:

# Transformation Operators
1. map
		-
2. flatMap
    
    - **`map`:** Transforms the elements emitted by a publisher using a provided function.
    - **`flatMap`:** Transforms the elements emitted by a publisher into other publishers, flattening the result into a single sequence.
    - **`switchMap`:** Similar to `flatMap`, but cancels the previous inner publisher when a new element arrives.
    - **`concatMap`:** Similar to `flatMap`, but maintains the order of the elements.
2. **Filtering Operators:**
    
    - **`filter`:** Filters elements emitted by a publisher based on a predicate.
    - **`distinct`:** Suppresses duplicate elements emitted by a publisher.
    - **`take`:** Takes a specified number of elements from the start of a sequence.
3. **Combining Operators:**
    
    - **`merge`:** Combines multiple publishers into a single publisher by merging their emissions.
    - **`concat`:** Concatenates the emissions of multiple publishers, maintaining the order.
    - **`zip`:** Combines the emissions of multiple publishers into a single publisher by pairing them based on their order.
4. **Utility Operators:**
    
    - **`doOnNext`, `doOnError`, `doOnComplete`:** Allows performing side-effects for each element, error, or completion event.
    - **`delay`:** Introduces a delay before emitting elements.
5. **Error Handling Operators:**
    
    - **`onErrorResume`:** Allows the publisher to resume emitting items when an error occurs.
    - **`onErrorReturn`:** Replaces an error with a default value.
    - **`retry`:** Re-subscribes to a publisher if an error occurs, up to a specified number of retries.
    - `switchIfEmpty` - operator is used to switch to an alternative Mono if the source Mono is empty
1. **Backpressure Handling Operators:**
    
    - **`onBackpressureBuffer`, `onBackpressureDrop`, `onBackpressureLatest`:** Control strategies for handling backpressure.
7. **Conditional Operators:**
    
    - **`takeWhile`, `skipWhile`:** Takes or skips elements while a given condition holds true.
8. **Mathematical and Aggregate Operators:**
    
    - **`count`, `sum`, `average`:** Perform mathematical operations on the elements.
    - **`reduce`:** Applies a binary operator to the elements and returns a single result.


## take 

Take only the first N values from this Flux, if available.




| Operator | Description |
| ---- | ---- |
| filter |  |
| map |  |
| flatMap |  |
| take |  |
| delayElements |  |
| doHooks |  |
| onError |  |
| timeout |  |
| defaultIfEmpty |  |
| switchIfEmpty |  |
| handle |  |
| limitRate |  |
| transform |  |
| switchOnFirst |  |
| flatMap |  |
|  |  |

