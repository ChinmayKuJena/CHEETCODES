
---

# **JAVA 8**

## Java 8 Features
1. Lambda Expression
2. Stream API
3. Date & Time API
4. Base64 Encode & Decode
5. Method & Constructor Reference
6. Default & Static Methods in Interface
7. Functional Interface
8. Optional Class

## Lambda Expression

### Anonymous Functions
- **No Name**: Lambda expressions do not have a name.
- **No Modifier**: No access modifiers are required.
- **No Return Type**: The return type is inferred from the context.

## Functional Interface

### Characteristics
- **One Abstract Method**: Functional interfaces must have exactly one abstract method.
- **Multiple Methods**: Having more than one abstract method results in a compile-time error.

**Example:**

**Error Example for Multiple Methods**
```java
@FunctionalInterface
interface MyInterface {
    void hy();
    void anotherMethod(); // Error: Multiple abstract methods
}
```

**Direct Implementation of Functional Interface**
```java
package Work;

public class Main {
    public static void main(String[] args) {
        MyInterface i = new MyInterface() {
            @Override
            public void hy() {
                System.out.println("hy i am anonymous");
            }
        };
        
        MyInterface i2 = new MyInterface() {
            @Override
            public void hy() {
                System.out.println("hy i am anonymous 2");
            }
        };
        
        i.hy();
        i2.hy();
    }
}
```

### Lambda Usage (For Functional Interfaces)
- Lambda expressions provide a more concise way to implement functional interfaces with a single abstract method.

**Example:**
```java
package Lambda;

public class Main {
    public static void main(String[] args) {
        MyInterface i = () -> System.out.println("1st lambda");
        i.hy();
    }
}
```

### Lambda Usage for Threads
- Lambda expressions simplify thread creation and execution by providing a concise way to define `Runnable`.

**Example:**
```java
package Thread;

public class ThreadMain {
    public static void main(String[] args) {
        Runnable thread1 = () -> {
            for (int i = 0; i < 11; i++) {
                System.out.println(i);
                try {
                    Thread.sleep(1000);
                    System.out.println("Wakeup");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };
        
        Thread thread = new Thread(thread1);
        thread.setName("BOOO");
        thread.start();
    }
}
```

## Filtering Strings Using Lambda
- Lambda expressions can be used with the Stream API to filter and manipulate collections.

**Example:**
```java
package Practice;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class FilteringStr {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "avocado");
        List<String> filter = words.stream()
                .filter(word -> word.startsWith("a"))
                .collect(Collectors.toList());
        System.out.println(filter); // Output: [apple, avocado]
    }
}
```

## Sorting List
- Lambda expressions simplify sorting operations when used with the Stream API.

**Example:**
```java
package Practice;

import java.util.Arrays;
import java.util.List;

public class SortingStr {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "kiwi", "cherry");
        words.stream()
                .sorted((a, b) -> Integer.compare(a.length(), b.length()))
                .forEach(System.out::println);
    }
}
```

## Custom Functional Interfaces

### Defining Custom Functional Interfaces
- You can define your own functional interfaces using the `@FunctionalInterface` annotation.

**Example:**
```java
@FunctionalInterface
interface StringProcessor {
    String process(String input);
}

public class CustomFunctionalInterfaceExample {
    public static void main(String[] args) {
        StringProcessor toUpperCase = str -> str.toUpperCase();
        System.out.println(toUpperCase.process("hello")); // Output: HELLO
    }
}
```

## Functional Interfaces from `java.util.function`

### Built-In Functional Interfaces
Java 8 provides several built-in functional interfaces that are commonly used in functional programming:

- **`Function<T, R>`**: Represents a function that takes an argument of type `T` and produces a result of type `R`.

**Example:**
```java
import java.util.function.Function;

public class FunctionExample {
    public static void main(String[] args) {
        Function<Integer, Integer> square = x -> x * x;
        System.out.println(square.apply(4)); // Output: 16
    }
}
```

- **`UnaryOperator<T>`**: A specialization of `Function` where the input and output types are the same.

**Example:**
```java
import java.util.function.UnaryOperator;

public class UnaryOperatorExample {
    public static void main(String[] args) {
        UnaryOperator<Integer> increment = x -> x + 1;
        System.out.println(increment.apply(5)); // Output: 6
    }
}
```

- **`BinaryOperator<T>`**: A specialization of `BiFunction` where the input and output types are the same.

**Example:**
```java
import java.util.function.BinaryOperator;

public class BinaryOperatorExample {
    public static void main(String[] args) {
        BinaryOperator<Integer> add = (a, b) -> a + b;
        System.out.println(add.apply(5, 3)); // Output: 8
    }
}
```

- **`Predicate<T>`**: Represents a boolean-valued function of one argument.

**Example:**
```java
import java.util.function.Predicate;

public class PredicateExample {
    public static void main(String[] args) {
        Predicate<String> isNotEmpty = str -> !str.isEmpty();
        System.out.println(isNotEmpty.test("Hello")); // Output: true
    }
}
```

- **`Consumer<T>`**: Represents an operation that accepts a single input argument and returns no result.

**Example:**
```java
import java.util.function.Consumer;

public class ConsumerExample {
    public static void main(String[] args) {
        Consumer<String> print = str -> System.out.println(str);
        print.accept("Hello, Lambda!"); // Output: Hello, Lambda!
    }
}
```

- **`Supplier<T>`**: Represents a supplier of results, providing a result with no input.

**Example:**
```java
import java.util.function.Supplier;

public class SupplierExample {
    public static void main(String[] args) {
        Supplier<String> supplier = () -> "Hello, World!";
        System.out.println(supplier.get()); // Output: Hello, World!
    }
}
```

## Functional Programming
- **Concept**: Functional programming emphasizes the use of pure functions, immutability, and higher-order functions.

- **Higher-Order Functions**: Functions that can take other functions as parameters or return functions.

**Example:**
```java
import java.util.function.Function;

public class HigherOrderFunctionExample {
    public static <T, R> R applyFunction(T value, Function<T, R> function) {
        return function.apply(value);
    }

    public static void main(String[] args) {
        Function<Integer, Integer> multiplyByTwo = x -> x * 2;
        Function<Integer, Integer> addFive = x -> x + 5;
        Function<Integer, Integer> combinedFunction = multiplyByTwo.andThen(addFive);
        System.out.println(applyFunction(3, combinedFunction)); // Output: 11
    }
}
```

---

