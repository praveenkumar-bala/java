
The **Supplier Interface** is a part of the **java.util.function** package

It represents a function which does not take in any argument but produces a value of type T. Hence this functional interface takes in only one generic namely:-

- **T**: denotes the type of the result

```
# import java.util.function.Supplier;

Supplier<String> name = new Supplier<String>() {  
    @Override  
    public String get() {  
        return "Praveen";  
    }  
};  
System.out.println(name.get());  
  
/**  
 * Using Lambda */// This function returns a random value. Supplier<Double> randomValue = () -> Math.random();  
  
// Print the random value using get() System.out.println(randomValue.get());

```