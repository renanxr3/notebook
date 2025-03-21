
# C#

MSIL = Microsoft Intermediate Language

NuGet.org

---

Sure thing, Renan! Let's dive into it.

### Classes
Classes are reference types and are used for complex data structures. They can contain data members (fields) and functions (methods). They support inheritance, meaning you can create a new class based on an existing class.

**Example:**
```csharp
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }

    public void Introduce()
    {
        Console.WriteLine($"Hi, I am {FirstName} {LastName}");
    }
}
```

### Structs
Structs are value types and are typically used for smaller data structures that contain primarily data. Unlike classes, structs do not support inheritance (other than from interfaces).

**Example:**
```csharp
public struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public void Display()
    {
        Console.WriteLine($"Point at ({X}, {Y})");
    }
}
```

### Records
Records are a reference type introduced in C# 9.0. They are intended for immutable data and provide built-in functionality for value equality (i.e., two records with the same data are considered equal).

**Example:**
```csharp
public record PersonRecord(string FirstName, string LastName);

var person1 = new PersonRecord("John", "Doe");
var person2 = new PersonRecord("John", "Doe");

Console.WriteLine(person1 == person2); // Output: True
```

**Key Differences:**
- **Classes**: Used for complex data structures, support inheritance, reference type.
- **Structs**: Used for small data structures, do not support inheritance, value type.
- **Records**: Used for immutable data, provide value equality, reference type.

I hope this clears up the differences for you! Feel free to ask if you have any more questions about C#. Happy coding! 😊


---

## Variable Declaration

Certainly! In C#, when declaring properties, you can use various accessors to control how the property is accessed and modified. Here's a summary of the main options:

1. **`get` and `set`:**  
   Allows both reading and modifying the property.
   ```csharp
   public string Name { get; set; }
   ```
   **Explanation:**  
   - `get` lets you retrieve the property's value.
   - `set` allows you to assign a new value to the property.

2. **`get` only (read-only property):**  
   A property that can only be read but not written to directly.
   ```csharp
   public string Name { get; }
   ```
   **Explanation:**  
   - You must initialize it in the constructor or via an initializer.

3. **`set` only (write-only property):**  
   A property that can only be written to.
   ```csharp
   private string _name;
   public string Name
   {
       set { _name = value; }
   }
   ```
   **Explanation:**  
   - You can assign a value, but you cannot directly access its value outside the class.

4. **`get` with `private set`:**  
   A read-only property outside the class but can be modified internally.
   ```csharp
   public string Name { get; private set; }
   ```
   **Explanation:**  
   - Often used for properties that should only be updated by the class itself.

5. **`get` with `init`:**  
   Enables setting the property during object initialization but prevents further modification.
   ```csharp
   public string Name { get; init; }
   ```
   **Explanation:**  
   - Introduced in C# 9. Ideal for immutable objects.
   - Can be set only when creating the object or via an object initializer.

6. **`get` with `protected set`:**  
   Allows derived classes to modify the property while keeping it read-only to others.
   ```csharp
   public string Name { get; protected set; }
   ```

7. **Calculated Property (`get` only without backing field):**  
   Computes the value dynamically instead of storing it.
   ```csharp
   public int Age => DateTime.Now.Year - BirthYear;
   ```
   **Explanation:**  
   - Often used when the property’s value is derived from other fields or properties.

These are the main options when defining properties in C#. If you're exploring `get` with `init`, it's perfect for creating immutable types that support initialization at the time of object creation. Here's an example:

```csharp
public class Person
{
    public string Name { get; init; }
    public int Age { get; init; }
}

// Usage:
var person = new Person { Name = "Renan", Age = 25 };
// person.Name = "John"; // This would throw an error.
```

Let me know which of these intrigues you the most or if you'd like to dive deeper into any of them!

