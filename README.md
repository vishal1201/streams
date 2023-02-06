# Java Streams
My take on learning Java Streams by doing.
Trying to collate all helpful snippets for future reference.
Feel free to use/fork/share. 

---

> Boilerplate code
>
> We'll use the following code later to perform some of our streams operations

```java
class Employee {
    private int ID;
    private String name;
    private double salary;
    Employee (int ID, String name, double salary) {
        this.ID = ID;
        this.name = name;
        this.salary = salary;
    }

    public double getSalary() {
        return salary;
    }

    public void incrementSalary(double percentage) {
        this.salary += (this.salary * percentage) / 100;
    }
}

class Main {
    public static void main(String[] args) {
        Employee[] employees = {
                new Employee(1, "Mickey Mouse", 100000.0),
                new Employee(2, "Donald Duck", 200000.0),
                new Employee(3, "Goofy Goo", 300000.0)
        };

        Employee brotherBear = new Employee(4, "Brother Bear", 5000.0);
        Employee mufasa = new Employee(5, "Mufasa - The Lion King", 500000.0);
    }
}
```
---

## Java Streams Creation
Following are some snippets to help create streams.
<br></br>

#### Stream of Employees from an existing array
```java
Stream<Employee> employeeStream = Stream.of(employees);
```

#### Stream of employees from an existing list
```java
List<Employee> empList = Arrays.asList(employees);
Stream<Employee> employeeStream = empList.stream();
```

#### Stream from a list of individual Employee objects.
```java
Stream<Employee> employeeStream = Stream.of(brotherBear, mufasa);
```

#### Using Stream.Builder to build a stream of Employee objects
```java
Stream.Builder<Employee> builder = Stream.builder();

builder.accept(brotherBear);
builder.accept(mufasa);
builder.accept(new Employee(6, "SherKhan", 450000.0));

Stream<Employee> employeeStream = builder.build();
```

---

## Java Streams Operations
Following are some snippets of available operations on Java Streams and their usages.
<br></br>

#### forEach
Loop over stream element and call the supplied function over each. `forEach()` is a terminal operation, i.e. once `forEach()`
is called, the stream is considered to be consumed.

```java
List<Employee> empList = Arrays.asList(employees);
empList.stream().forEach(e -> e.incrementSalary(10.0));
```

> **_NOTE:_**  Stream can be consumed only once. If attempted to consume after being consumed, the following exception is thrown
> 
> **IllegalStateException: stream has already been operated upon or closed.**


#### map
`map()` applies the supplied function to each element of the current stream
and returns a new stream. The resultant stream can be of the same or different type.

```java
List<Employee> empList = Arrays.asList(employees);
List<Double> salaries = empList.stream().
        .map(Employee::getSalary).collect(Collectors.toList());
```

```java
// stream of integers returned as stream of squared versions of themselves.
Stream<Integer> squared = Stream.of(1, 2, 3, 4, 5).map(x -> x * x);
```

#### collect
Once all the stream processing is done, we can use the `collect()`
with a suitable collector option.
We saw in the previous example how we used collect with toList collector
to collect stream of `double salary` into the `List<Double> salaries`.
The strategy for collection is provided via the `Collector` interface.

#### filter
`filter()` as the name suggests, helps filter a given stream. It does so,
by passing the elements of input stream via a Predicate, that
evaluates to either `true / false` based on a condition on the element.
This also means, that `flter()` generates a stream of the same type of elements.

```java
List<Employee> employeesWithSalariesUnder200K = empList.stream()
                .filter(e -> e.getSalary() < 200000)
                .collect(Collectors.toList());
```

#### findFirst
`findFirst()` returns an Optional for the first entry in the stream; the Optional can, of course, be empty.

```java
Optional<Employee> employee = empList.stream().findFirst();
```

#### toArray
`collect()` is used to collect the stream into a Collection. If we need to get an array out of the stream, we can simply use `toArray()`.

```java
Employee[] employees = empList.stream().toArray(Employee[]::new);
```

#### flatMap
`flatMap()` helps us flatten a complex stream such as below. We have a `List<List<Stream>>>`;
flatMap would help us flatten it to a `Stream<String>` which can be collected into a `List<String>`.
`map()` takes one input, generates one output. `flatMap()` takes one input and generates
zero or more outputs.

```java
List<List<String>> couples = Arrays.asList(
        Arrays.asList("Donald Duck", "Daisy Duck"),
        Arrays.asList("Mickey Mouse", "Minnie Mouse"),
        Arrays.asList("Nobita", "Shizuka"));

List<String> employees = couples.stream()
                .flatMap(Collection::stream)
                .collect(Collectors.toList());
```

#### peek
`peek()` is an intermediate operation that helps perform a function
over each element of a stream.

---

