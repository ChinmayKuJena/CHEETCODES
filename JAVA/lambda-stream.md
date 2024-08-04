
---

# Java 8: Lambda Expressions and Stream API

## Lambda Expressions

### What is a Lambda Expression?

A lambda expression is a concise way to represent an anonymous function that can be used to create instances of functional interfaces (interfaces with a single abstract method). Lambda expressions enable you to write more readable and maintainable code by providing a clear and succinct way to represent instances of single-method interfaces.

### Syntax

The basic syntax of a lambda expression is:

```java
(parameters) -> expression
```

Or, if there are multiple statements:

```java
(parameters) -> {
    // statements
}
```

### Components

- **Parameters**: The input parameters to the lambda expression. If there is only one parameter, the parentheses can be omitted.
- **Arrow Operator (`->`)**: Separates the parameters from the body of the lambda expression.
- **Body**: Contains the expression or statements that are executed when the lambda expression is invoked.

### Example: Basic Lambda Expression

**Functional Interface Definition:**

```java
@FunctionalInterface
interface MyFunction {
    int apply(int x);
}
```

**Using Lambda Expression:**

```java
public class LambdaExample {
    public static void main(String[] args) {
        MyFunction square = x -> x * x;
        int result = square.apply(5);
        System.out.println(result); // Output: 25
    }
}
```

### Example: Lambda with Standard Functional Interfaces

Java 8 provides several built-in functional interfaces in the `java.util.function` package:

- **`Function<T, R>`**: Takes an argument of type `T` and returns a result of type `R`.

  ```java
  import java.util.function.Function;

  public class FunctionExample {
      public static void main(String[] args) {
          Function<Integer, Integer> square = x -> x * x;
          int result = square.apply(4);
          System.out.println(result); // Output: 16
      }
  }
  ```

- **`Predicate<T>`**: Takes an argument of type `T` and returns a boolean value.

  ```java
  import java.util.function.Predicate;

  public class PredicateExample {
      public static void main(String[] args) {
          Predicate<String> isEmpty = str -> str.isEmpty();
          boolean result = isEmpty.test("");
          System.out.println(result); // Output: true
      }
  }
  ```

- **`Consumer<T>`**: Takes an argument of type `T` and performs an operation with no return value.

  ```java
  import java.util.function.Consumer;

  public class ConsumerExample {
      public static void main(String[] args) {
          Consumer<String> print = str -> System.out.println(str);
          print.accept("Hello"); // Output: Hello
      }
  }
  ```

- **`Supplier<T>`**: Provides a result of type `T` with no input.

  ```java
  import java.util.function.Supplier;

  public class SupplierExample {
      public static void main(String[] args) {
          Supplier<String> getGreeting = () -> "Hello, World!";
          String greeting = getGreeting.get();
          System.out.println(greeting); // Output: Hello, World!
      }
  }
  ```

- **`UnaryOperator<T>`**: A `Function` that returns a result of the same type as its input.

  ```java
  import java.util.function.UnaryOperator;

  public class UnaryOperatorExample {
      public static void main(String[] args) {
          UnaryOperator<Integer> increment = x -> x + 1;
          int result = increment.apply(5);
          System.out.println(result); // Output: 6
      }
  }
  ```

- **`BinaryOperator<T>`**: A `BiFunction` where both arguments and the result are of the same type.

  ```java
  import java.util.function.BinaryOperator;

  public class BinaryOperatorExample {
      public static void main(String[] args) {
          BinaryOperator<Integer> add = (a, b) -> a + b;
          int result = add.apply(5, 3);
          System.out.println(result); // Output: 8
      }
  }
  ```

## Stream API

### What is the Stream API?

The Stream API in Java 8 allows you to process sequences of elements (like collections) in a functional style. Streams provide a way to perform complex data manipulations, such as filtering, mapping, and reducing, with a more declarative approach.

### Key Concepts

- **Stream**: A sequence of elements supporting sequential and parallel aggregate operations.
- **Intermediate Operations**: Operations that transform a stream into another stream. They are lazy and are executed only when a terminal operation is invoked. Examples: `filter()`, `map()`, `sorted()`.
- **Terminal Operations**: Operations that produce a result or a side-effect and terminate the stream. Examples: `collect()`, `forEach()`, `reduce()`.

### Example: Filtering a List

**Filtering Strings That Start with a Specific Letter**

```java
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

### Example: Sorting a List

**Sorting Strings by Length**

```java
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

### Example: Mapping Elements

**Converting All Strings to Upper Case**

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class MappingExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");
        List<String> upperCaseWords = words.stream()
                .map(String::toUpperCase)
                .collect(Collectors.toList());
        System.out.println(upperCaseWords); // Output: [APPLE, BANANA, CHERRY]
    }
}
```

### Example: Reducing Elements

**Finding the Sum of All Elements**

```java
import java.util.Arrays;
import java.util.List;

public class ReducingExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        int sum = numbers.stream()
                .reduce(0, (a, b) -> a + b);
        System.out.println(sum); // Output: 15
    }
}
```

### Example: Collecting Results

**Counting the Number of Elements**

```java
import java.util.Arrays;
import java.util.List;

public class CollectingExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");
        long count = words.stream().count();
        System.out.println(count); // Output: 3
    }
}
```

### Example: Parallel Streams

**Using Parallel Streams for Performance Improvement**

```java
import java.util.Arrays;
import java.util.List;

public class ParallelStreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        numbers.parallelStream()
                .map(n -> n * n)
                .forEach(System.out::println);
    }
}
```

---

