---
title: '"Java - Lambda and Functional Interfaces"'
draft: 
tags:
  - java
  - lambdas
  - jdk
  - functionalprograming
  - functional-interface
---
# Lambdas

Lambdas were replacement of anonymous class. They give is the power of treating code as data. That means, now a function can take another function as input parameter. This makes things easy in a lot of ways. Specially, when we need to do data transformations. We can apply single filter and apply it on a collection and then return the filtered response. 


Lambdas are functional interface (Any interface that has only one abstract method, it can have one or more default and static methods in it). If we don't have lambdas we need to use anonymous class that will look like this. 

```
printObject(myCollection, 
			new CheckMyCollection() {
				public boolean test(Person p) {
					return p.getGender() == Persopn.Sex.MALE
					&& p.getAge() >=16 && p.getAge() <=25;	
				}
			}
		);
```

Lambda version: 

```
printObject(
	roster, (Person p) -> p.getGender() == Person.Sex.MALE && p.geAge() >=16 && p.getAge() <=25
);
```

# Functional Interfaces
#### Predicate Interface

It only gives boolean results.

Jshell: 
```
Predicate<String> checkSomething = x -> x.length() > 2;
System.out.println(checkSomething.test("duck"));



// Another example 

Predicate<Integer> isPositive = x -> x > 0;
Predicate<Integer> isEven = x -> x % 2 == 0;
Predicate<Integer> isPositiveAndEven = isPositive.and(isEven);


List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

numbers.stream().filter(isEven).collect(Collectors.toList());
System.out.println(evenNumbers); // [2, 4, 6]


// Can be done in a single line. 
Arrays.asList(1, 2, 3, 4, 5, 6).stream().filter(isEven).collect(Collectors.toList());



```

#### Consumer Interface

This is used for modify data, it does not return anything. Good for things like data transformations.

```

Consumer<String> printConsumer = str -> System.out.println("Print: " + str);

Consumer<String> lengthConsumer = str -> System.out.println("Length: " + str.length());
Consumer<String> combinedConsumer = printConsumer.andThen(lengthConsumer);


combinedConsumer.accept("Hello, World!");

List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

Consumer<String> uppercaseConsumer = str -> System.out.println(str.toUpperCase());

names.forEach(uppercaseConsumer);
```


#### Function Interface

This is used for transformations and mappings. 


```
Function<String, Integer> lengthFunction = str -> str.length();
lengthFunction.apply("Hello")

```

```

Function<String, Integer> lengthFunction = str -> str.length();

        Function<Integer, Integer> squareFunction = x -> x * x;

        Function<String, Integer> composedFunction = lengthFunction.andThen(squareFunction);
        
        composedFunction.apply("Hello")
```
#### Supplier Interface

 This does not take any arguments and only returns a result. It’s often used in scenarios where values need to be lazily evaluated or deferred until needed.


```
public class DeferredComputationExample {

    private static Map<Integer, Long> fibCache = new HashMap<>();

  

    public static void main(String[] args) {

        Supplier<Long> fibonacciSupplier = () -> {

            return computeFibonacci(10); // Compute Fibonacci number for n=10

        };

  

        // No computation happens until get() is called

        System.out.println("Before get()");

        System.out.println("Fibonacci number: " + fibonacciSupplier.get()); // Compute Fibonacci number when needed

        System.out.println("After get()");

    }

  

    // Compute Fibonacci number using memoization

    private static long computeFibonacci(int n) {

        if (n <= 1) {

            return n;

        }

  

        // Check cache first

        if (fibCache.containsKey(n)) {

            return fibCache.get(n);

        }

  

        // Compute recursively and store in cache

        long result = computeFibonacci(n - 1) + computeFibonacci(n - 2);

        fibCache.put(n, result);

        return result;

    }

}
```

#### Predicate Interface

It is often used for conditions where you need to test objects and return a boolean value based on some criteria.

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
Predicate<Integer> isEven = n -> n % 2 == 0;
numbers.stream().filter(isEven).collect(Collectors.toList());                                   
```

---
# Lambdas operations. 

#### **forEach**

```

import java.util.Arrays;

import java.util.List;

  

public class StreamExamples {

    public static void main(String[] args) {

        List<String> languages = Arrays.asList("Java", "Python", "JavaScript", "C#", "Ruby");

  

        // Using forEach to print each element in the list

        languages.stream()

                 .forEach(lang -> System.out.println("Language: " + lang));

    }

}
```

### **filter**
```
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExamples {
    public static void main(String[] args) {
        List<String> languages = Arrays.asList("Java", "Python", "JavaScript", "C#", "Ruby");

        // Using filter to select languages starting with 'J'
        List<String> filteredLanguages = languages.stream()
                                                  .filter(lang -> lang.startsWith("J"))
                                                  .collect(Collectors.toList());

        System.out.println("Filtered languages: " + filteredLanguages); // Output: [Java, JavaScript]
    }
}

```

### **Map**

```
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExamples {
    public static void main(String[] args) {
        List<String> languages = Arrays.asList("Java", "Python", "JavaScript", "C#", "Ruby");

        // Using map to transform each language to its length
        List<Integer> languageLengths = languages.stream()
                                                 .map(lang -> lang.length())
                                                 .collect(Collectors.toList());

        System.out.println("Language lengths: " + languageLengths); // Output: [4, 6, 10, 2, 4]
    }
}
```

### **collect**

```
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExamples {
    public static void main(String[] args) {
        List<String> languages = Arrays.asList("Java", "Python", "JavaScript", "C#", "Ruby");

        // Using collect to join all languages into a single comma-separated string
        String joinedLanguages = languages.stream()
                                         .collect(Collectors.joining(", "));

        System.out.println("Joined languages: " + joinedLanguages); // Output: Java, Python, JavaScript, C#, Ruby
    }
}
```


### **flatMap**

It’s particularly useful when dealing with nested collections or when you want to transform and combine elements in a nested structure.

```
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExamples {
    public static void main(String[] args) {
        List<List<Integer>> nestedLists = Arrays.asList(
                Arrays.asList(1, 2, 3),
                Arrays.asList(4, 5, 6),
                Arrays.asList(7, 8, 9)
        );

        // Using flatMap to flatten the nested lists into a single stream of integers
        List<Integer> flattenedList = nestedLists.stream()
                                                 .flatMap(List::stream)
                                                 .collect(Collectors.toList());

        System.out.println("Flattened list: " + flattenedList); // Output: [1, 2, 3, 4, 5, 6, 7, 8, 9]
    }
}
```

### **reduce**

```
import java.util.Arrays;
import java.util.List;

public class StreamExamples {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        // Using reduce to calculate the sum of numbers in the stream, Integer.MIN_VALUE is from where to start reducing.
        int sum = numbers.stream()
                         .reduce(Integer.MIN_VALUE, (a, b) -> a + b);

        System.out.println("Sum of numbers: " + sum); // Output: 15
    }
}
```

### **sorted**

```
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExamples {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("banana", "apple", "cherry", "date");

        // Using sorted to sort the words alphabetically
        List<String> sortedWords = words.stream()
                                       .sorted()
                                       .collect(Collectors.toList());

        System.out.println("Sorted words: " + sortedWords); // Output: [apple, banana, cherry, date]
    }
}
```

### **distinct**

```
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExamples {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 3, 4, 5);

        // Using distinct to filter unique numbers
        List<Integer> uniqueNumbers = numbers.stream()
                                             .distinct()
                                             .collect(Collectors.toList());

        System.out.println("Unique numbers: " + uniqueNumbers); // Output: [1, 2, 3, 4, 5]
    }
}
```

### **Limt and skip**

```

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExamples {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // Using limit and skip for paging through the list
        List<Integer> page = numbers.stream()
                                    .skip(3)     // Skip the first 3 elements
                                    .limit(3)    // Limit to 3 elements
                                    .collect(Collectors.toList());

        System.out.println("Paged list: " + page); // Output: [4, 5, 6]
    }
}
```