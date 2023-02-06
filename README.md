# Java Streams
My take on learning Java Streams by doing.
Trying to collate all helpful snippets for future reference.
Feel free to use/fork/share. 

---

<b>Boilerplate code</b>

We'll use the following code later to perform some of our streams operations

```java
public record Employee (int ID, String name, double salary) {}

Employee[] employees = {
    new Employee(1, "Mickey Mouse", 100000.0),
    new Employee(2, "Donald Duck", 200000.0),
    new Employee(3, "Goofy Goo", 300000.0)
};

Employee brotherBear = new Employee(4, "Brother Bear", 5000.0);
Employee mufasa = new Employee(5, "Mufasa - The Lion King", 500000.0);
```
---

## Java Streams Creation
Following are some snippets to help create streams.

### Stream of Employees from an existing array
```java
Stream<Employee> employeeStream = Stream.of(employees);
```

### Stream of employees from an existing list
```java
List<Employee> empList = Arrays.asList(employees);
Stream<Employee> employeeStream = empList.stream();
```

### Stream from a list of individual Employee objects.
```java
Stream<Employee> employeeStream = Stream.of(brotherBear, mufasa);
```

### Using Stream.Builder to build a stream of Employee objects
```java
Stream.Builder<Employee> builder = Stream.builder();

builder.accept(brotherBear);
builder.accept(mufasa);
builder.accept(new Employee(6, "SherKhan", 450000.0));

Stream<Employee> employeeStream = builder.build();
```