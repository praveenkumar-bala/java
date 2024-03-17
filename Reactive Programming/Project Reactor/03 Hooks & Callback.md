
### Hooks:

Hooks in Project Reactor are callback mechanisms that allow you to customize the behavior of certain events in the reactive execution. The hooks are global, meaning they apply to all Reactor instances in the JVM. The hooks provided by Project Reactor include:

1. **OnOperatorHook:**
    
    - Allows you to intercept calls to operators, giving you the ability to customize or log the behavior of operators.
2. **OnLastOperator:**
    
    - Similar to `OnOperatorHook`, but specific to the last operator in the chain.
3. **OnEachOperator:**
    
    - Provides a callback for each operator in the chain.
4. **OnDiscardHook:**
    
    - Allows you to be notified when elements are discarded, providing an opportunity for resource cleanup or logging.
5. **OnNextHook:**
    
    - Notifies you when an element is about to be delivered to a subscriber.
6. **OnErrorHook:**
    
    - Provides a callback for handling errors that occur during reactive processing.
7. **OnErrorDroppedHook:**
    
    - Notifies you when an error is about to be dropped, providing an opportunity to log or handle the dropped error.
8. **OnCompleteHook:**
    
    - Callback for handling the completion of a reactive sequence.
9. **OnAfterTerminateHook:**
    
    - Invoked after termination, whether due to completion or error.

To use hooks, you can register them using `Hooks.onOperator(...)`, `Hooks.onLastOperator(...)`, etc.

### Example of Using Hooks:


```
Hooks
.onOperator("myCustomOperator", 
	operator -> {     // Custom logic for intercepting the operator    
		System.out.println("Intercepting operator: " + operator.getClass());     
		return operator; 
	});  

Flux.just("A", "B", "C")     
.map(String::toUpperCase)     
.subscribe(System.out::println);
```

In this example, the `Hooks.onOperator` hook is used to intercept calls to the `map` operator. You can register hooks with specific names and types to target specific operators.

### Callbacks:

In addition to hooks, Project Reactor provides various callbacks that you can use within your reactive code to customize behavior at specific points. For example:

- **`doOnNext`:**
    
    - Allows you to perform an action when an element is emitted.
- **`doOnError`:**
    
    - Allows you to perform an action when an error occurs.
- **`doOnComplete`:**
    
    - Allows you to perform an action when the sequence completes successfully.
- **`doOnSubscribe`:**
    
    - Allows you to perform an action when a subscription occurs.
- **`doFinally`:**
    
    - Allows you to perform an action when the sequence terminates, whether due to completion, error, or cancellation.

### Example of Using Callbacks:

```
Flux.just("A", "B", "C")     
	.map(String::toUpperCase)     
	.doOnNext(data -> System.out.println("Received: " + data))     
	.doOnError(error -> System.err.println("Error: " + error))     
	.doOnComplete(() -> System.out.println("Completed"))     
	.subscribe();`
```
In this example, `doOnNext`, `doOnError`, and `doOnComplete` are used as callbacks to customize behavior when elements are emitted, an error occurs, or the sequence completes.

