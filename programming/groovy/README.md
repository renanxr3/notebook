## Groovy

### Annotations

Groovy supports annotations like Java does. Some examples:
  - **@ToString:** Automatically generates the toString method for the class.
  - **@EqualsAndHashCode:** Generates the equals and hashCode methods.
  - **@TupleConstructor:** Creates constructors for all possible combinations of class properties.
  - **@Canonical:** Aggregates the functionality of @ToString, @EqualsAndHashCode, and @TupleConstructor.

### Closure 

Closure is an anonymous block of code you can define in middle of your code, and can be sent as a parameter to another function, similar to a callback function in Javascript.

This code demonstrates defining and using closures, handling null values, and passing closures as parameters to methods.

```groovy
// Define the Closure
def personToString = { Person person ->
  person.toString()
}

// Method to handle the person
def handlePerson(Closure closure, Person person) {
  if (person == null) {
    throw new RuntimeException("Person cannot be null")
  }
  closure(person)
}

// Create a new Person instance
Person johnDoe = new Person("John", "Doe")

// Invoke the Closure using the handlePerson method
handlePerson(personToString, johnDoe)

// Another Closure to print the full name of a person
def printFullName = { Person person ->
  println("${person.firstName} ${person.lastName}")
}

// Invoke the new Closure
handlePerson(printFullName, johnDoe)
```

### Collections

```groovy
// Creating a list
def allPersons = ['John Doe', 'Jane Smith', 'Sam Hill']

// Querying the list
println allPersons.size() // Output: 3
println allPersons[0] // Output: John Doe

// Iterating over the list using each
allPersons.each { println it }

// Iterating with index using eachWithIndex
allPersons.eachWithIndex { person, index -> println "$index: $person" }

// Finding a specific element
def person = allPersons.find { it == 'Sam Hill' }
println person // Output: Sam Hill

// Transforming elements using collect
def ages = [25, 30, 35]
def isYoungerOrEqualTo30 = ages.collect { it <= 30 }
println isYoungerOrEqualTo30 // Output: [true, true, false]

// Sorting the list
def sortedAges = ages.sort()
println sortedAges // Output: [25, 30, 35]
```

---

### Writing Files

```groovy
class HelloWorld {
    static void main(String[] args) {
        // Create a file and populate contents
        File textFile = new File("resources/mary-hill.txt")
        textFile.withWriter('UTF-8') { writer ->
            writer.writeLine("Mary")
            writer.writeLine("Hill")
            writer.writeLine("30")
        }

        // Appending contents to a file
        textFile.append("1")
        textFile << "2"

        // Serializing an object to a file
        Person thomasMarks = new Person("Thomas", "Marks", 21)
        File binFile = new File("resources/thomas-marks.bin")

        binFile.withObjectOutputStream { out ->
            out.writeObject(thomasMarks)
        }
    }
}

import groovy.transform.Canonical

@Canonical
class Person implements Serializable {
    String firstName
    String lastName
    int age
}
```

### Reading Files

```groovy
class Main {
    static void main(String[] args) {
        // Read numbers from files and store them in List
        List<Integer> allNumbers = readAllNumbers()

        // Find the highest number
        Integer maxNumber = allNumbers.max()
        assert maxNumber == 2044

        // Create the sum of all numbers
        Integer sum = allNumbers.sum()
        assert sum == 8180
    }

    private static List<Integer> readAllNumbers() {
        File resourcesDir = new File("resources")
        List<Integer> allNumbers = []

        resourcesDir.eachFile { file ->
            file.eachLine { line ->
                if (line.isNumber()) {
                    allNumbers << line.toInteger()
                }
            }
        }

        allNumbers
    }
}
```

