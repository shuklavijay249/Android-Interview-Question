# Android-Interview-Question-Core-Topics


## Root Class
- In Java, the root class is **Object**.
- In Kotlin, the root class is **Any**.

---
Here's an explanation of **Heap Memory** and **Stack Memory** in the context of Android, formatted for GitHub's markdown:

```markdown
# Heap Memory vs Stack Memory in Android

In Android development, understanding **Heap Memory** and **Stack Memory** is crucial for efficient memory management, especially for optimizing app performance and avoiding memory leaks.

## 1. Stack Memory in Android
Stack memory in Android is used for **static memory allocation** and stores:
- Local variables within functions or methods
- Primitive data types (e.g., `int`, `char`, `boolean`)
- Function call information like return addresses and parameters

### Key Characteristics:
- **Size**: Fixed and limited for each thread. The system assigns memory automatically.
- **Lifetime**: The data is only valid during the execution of a method and is cleared once the method finishes.
- **Access speed**: Fast due to the LIFO (Last In, First Out) structure.

### Example in Android (Java/Kotlin):
```kotlin
fun myFunction() {
    val x = 10 // Allocated in stack memory
    val y = 20 // Allocated in stack memory
}
```

In this example, `x` and `y` are local variables that exist within the method and are automatically cleared from the stack when `myFunction()` completes.

### Advantages of Stack Memory in Android:
- **Fast access**: Since it works in a LIFO manner, stack memory operations are quick.
- **Automatic management**: No need for explicit memory management; Android manages the stack automatically.

### Disadvantages:
- **Limited size**: Stack memory is limited in size per thread, and excessive use can lead to a `StackOverflowError`.
- **Scope limitations**: Variables stored in the stack only exist within the method or function where they are defined.

---

## 2. Heap Memory in Android
Heap memory in Android is used for **dynamic memory allocation** and stores:
- Objects (e.g., instances of classes)
- Large arrays and collections (like `ArrayList`, `HashMap`)
- Data that needs to persist across method calls

### Key Characteristics:
- **Size**: Larger but limited by the Android system based on the device's memory.
- **Garbage collection**: The Android runtime (ART/Dalvik) manages heap memory using garbage collection (GC), which automatically reclaims memory when objects are no longer in use.
- **Lifetime**: Data in heap memory remains allocated until garbage collection frees it.

### Example in Android (Java/Kotlin):
```kotlin
class Person {
    var name: String = ""
    var age: Int = 0
}

fun main() {
    val person = Person() // Allocated in heap memory
    person.name = "John"
    person.age = 25
}
```

Here, the `person` object is allocated in heap memory, and it stays there until it's no longer referenced and the garbage collector reclaims the memory.

### Advantages of Heap Memory in Android:
- **Flexibility**: Allows dynamic memory allocation and supports complex data structures like objects.
- **Persistence**: Objects can persist across multiple method calls, making it ideal for managing app state.

### Disadvantages:
- **Slower access**: Accessing heap memory is slower compared to stack memory due to the overhead of dynamic allocation.
- **Memory leaks**: Improper memory management (e.g., failing to release object references) can lead to memory leaks, where heap memory is not freed even when it's no longer needed.

---

## 3. Key Differences Between Stack and Heap Memory in Android

| Feature                | Stack Memory                      | Heap Memory                         |
|------------------------|-----------------------------------|-------------------------------------|
| **Usage**              | Stores method calls, local variables, and primitives | Stores objects and dynamic data    |
| **Access Speed**       | Faster (LIFO structure)            | Slower (dynamic allocation)         |
| **Size**               | Fixed size per thread              | Larger but managed by Android system|
| **Memory Management**  | Automatic (pushed/popped with method calls) | Managed via garbage collection     |
| **Error**              | `StackOverflowError` when exceeded | `OutOfMemoryError` when exceeded    |
| **Lifetime**           | Data cleared after method exits    | Data persists until garbage collected|

---

## 4. Common Memory Issues in Android

- **StackOverflowError**: This occurs when the stack memory limit is exceeded, usually due to deep or infinite recursion.
- **OutOfMemoryError (OOM)**: Happens when heap memory is exhausted. For example, this can occur if large bitmaps or arrays are loaded without proper memory management.

### Example of Memory Leak:
In Android, memory leaks often happen when an object holds a reference to an Activity or Context, preventing garbage collection. For example:
```kotlin
class MyClass {
    var context: Context? = null
}

fun someMethod() {
    val myClass = MyClass()
    myClass.context = this // Holding reference to an Activity context
}
```
In this case, the `context` can cause a memory leak if the activity is destroyed but the object still holds the reference.

---

## 5. Best Practices for Memory Management in Android
1. **Avoid memory leaks**: Be mindful of holding long-lived references to `Context` or `Activity` objects.
2. **Use weak references**: When necessary, use `WeakReference` to prevent memory leaks.
3. **Release resources**: Ensure you release or close resources (e.g., Bitmaps, Streams) when they are no longer needed.
4. **Use efficient data structures**: Use lightweight data structures and avoid unnecessarily large objects.
5. **Optimize bitmaps**: Use efficient loading techniques for large images (e.g., `BitmapFactory.Options` to scale images down).

---

Understanding the difference between **stack** and **heap memory** in Android is essential for optimizing memory usage and avoiding crashes due to memory-related issues.
```

## Interface and Abstract Class

In Android and Java/Kotlin development, **interfaces** and **abstract classes** are both used to define abstract behavior, but they have distinct differences in terms of functionality and use cases.

### **Differences Between Interface and Abstract Class**:

| Feature                    | **Interface**                           | **Abstract Class**                           |
|----------------------------|-----------------------------------------|----------------------------------------------|
| **Method Implementation**   | Cannot have method implementations (in Java before version 8). Java 8+ allows default methods. In Kotlin, interfaces can have default method implementations. | Can have both abstract methods and methods with implementations. |
| **Multiple Inheritance**    | A class can implement multiple interfaces. | A class can inherit from only one abstract class. |
| **Fields**                  | Can have constants (static final fields), but no instance variables. | Can have both constants and instance variables. |
| **When to Use**             | Use when you want to define a contract that multiple classes should follow, but they may not share common functionality. | Use when you want to define common behavior that all subclasses should inherit, with the ability to share code (method implementations). |
| **Accessibility Modifiers** | Methods in an interface are `public` by default. | Methods and fields can have any access modifier (`private`, `protected`, `public`). |
| **Constructor**             | Cannot have constructors. | Can have constructors, allowing you to initialize common fields for subclasses. |

---

### **Interface Example in Android**:

#### Scenario:
Imagine you have multiple different activities that all need to handle click events in a uniform way. Instead of duplicating click handling code, you can define an interface.

#### Step 1: Define an Interface
In this example, an interface called `OnClickListener` is defined. Any class implementing this interface must provide its own implementation of the `onClick()` method.

```kotlin
interface OnClickListener {
    fun onClick(view: View)
}
```

#### Step 2: Implement the Interface in a Class
Here, two classes (`ButtonClickHandler` and `ImageClickHandler`) implement the `OnClickListener` interface in their own way.

```kotlin
class ButtonClickHandler : OnClickListener {
    override fun onClick(view: View) {
        // Handle button click
        println("Button clicked!")
    }
}

class ImageClickHandler : OnClickListener {
    override fun onClick(view: View) {
        // Handle image click
        println("Image clicked!")
    }
}
```

### Why Use Interfaces?
- **Flexibility**: Different classes can implement the same interface in their own way.
- **Multiple Inheritance**: A class can implement multiple interfaces. For example, a class can implement both `OnClickListener` and another interface like `OnLongClickListener` to handle different user actions.

---

### **Abstract Class Example in Android**:

#### Scenario:
You want to create a base `Activity` that all other activities will extend. This base class will have some shared methods like setting up the toolbar or managing the lifecycle, but you also want each child class to provide its own specific functionality.

#### Step 1: Define an Abstract Class
Here, `BaseActivity` is an abstract class that provides common functionality for all activities. It has an abstract method `setupUI()` which child classes must implement.

```kotlin
abstract class BaseActivity : AppCompatActivity() {

    // Common method for setting up the toolbar
    fun setupToolbar(title: String) {
        supportActionBar?.title = title
    }

    // Abstract method that must be implemented by subclasses
    abstract fun setupUI()
}
```

#### Step 2: Implement the Abstract Class in a Child Class
The `MainActivity` class extends `BaseActivity` and must provide an implementation for the `setupUI()` method.

```kotlin
class MainActivity : BaseActivity() {

    override fun setupUI() {
        setupToolbar("Main Activity")
        // Additional UI setup
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        setupUI()  // Calls the abstract method implemented in this class
    }
}
```

### Why Use Abstract Classes?
- **Shared Code**: Abstract classes allow you to provide common functionality to multiple subclasses.
- **Partial Implementation**: You can provide partial implementation in an abstract class and force subclasses to implement the remaining functionality.
- **Complexity Management**: Useful when subclasses share a lot of code but still need to implement some specific functionality.

---

### **Use Case Example: Interface vs Abstract Class**

#### **Interface: Handling Multiple Input Types**
Let's say you're developing an app where different components handle different types of user input (e.g., click, swipe, long press). You could define separate interfaces:

```kotlin
interface OnClickListener {
    fun onClick(view: View)
}

interface OnSwipeListener {
    fun onSwipe(direction: String)
}

class ClickHandler : OnClickListener {
    override fun onClick(view: View) {
        // Handle click
    }
}

class SwipeHandler : OnSwipeListener {
    override fun onSwipe(direction: String) {
        // Handle swipe
    }
}
```

Since a class can implement multiple interfaces, you can easily define a class that handles both clicks and swipes.

#### **Abstract Class: Base Class for Activities**
In contrast, you might want to provide shared functionality like toolbar management or common lifecycle behavior across different activities in your app, but still allow each activity to implement its own UI setup.

```kotlin
abstract class BaseActivity : AppCompatActivity() {
    fun setupToolbar(title: String) {
        supportActionBar?.title = title
    }

    abstract fun setupUI()  // Forces subclass to define specific UI behavior
}
```

---

## OOPS Principles

Object-Oriented Programming (OOP) is a programming paradigm based on the concept of "objects," which can contain data in the form of fields (often known as attributes or properties) and code in the form of procedures (often known as methods). Here are the main principles of OOP:

### 1. **Encapsulation**
Encapsulation is the bundling of data (attributes) and methods (functions) that operate on the data into a single unit, or class. It restricts direct access to some of an object's components and can prevent the accidental modification of data. This is often achieved through access modifiers.

**Example in Kotlin:**
```kotlin
class Person(private var name: String) {
    fun getName(): String {
        return name
    }

    fun setName(newName: String) {
        name = newName
    }
}
```

**Example 2:**
```kotlin
class UserRepository {

    // Private Retrofit service instance
    private val userService: UserService by lazy {
        Retrofit.Builder()
            .baseUrl("https://example.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(UserService::class.java)
    }

    // Public method that can be called by other classes
    suspend fun getUser(userId: Int): User {
        return userService.getUser(userId) // Encapsulates API call
    }
}

class UserViewModel : ViewModel() {

    private val repository = UserRepository()

    private val _user = MutableLiveData<User>()
    val user: LiveData<User> get() = _user

    // Fetch user from repository
    fun fetchUser(userId: Int) {
        viewModelScope.launch {
            _user.value = repository.getUser(userId)
        }
    }
}
```

**Explanation of Encapsulation:**
- **Private members**: In the `UserRepository`, the details of the Retrofit initialization and the actual API call (`userService.getUser()`) are hidden from other parts of the app.
- **Public methods**: The `getUser()` method is public, exposing a simple interface for the ViewModel or other components to retrieve user data.

---

### 2. **Abstraction**
Abstraction is the concept of hiding the complex reality while exposing only the necessary parts. It helps to reduce programming complexity and increase efficiency. In Kotlin, abstraction can be achieved through abstract classes and interfaces.

**Example in Kotlin:**
```kotlin
abstract class Animal {
    abstract fun sound()
}

class Dog : Animal() {
    override fun sound() {
        println("Bark")
    }
}

class Cat : Animal() {
    override fun sound() {
        println("Meow")
    }
}
```

---

### 3. **Inheritance**
Inheritance is the mechanism by which one class can inherit the properties and methods of another class. This promotes code reuse and establishes a hierarchical relationship between classes. In Kotlin, you can create subclasses that extend a base class.

**Example in Kotlin:**
```kotlin
open class Vehicle {
    fun start() {
        println("Vehicle is starting")
    }
}

class Car : Vehicle() {
    fun honk() {
        println("Car is honking")
    }
}
```

---

### 4. **Polymorphism**
Polymorphism allows objects to be treated as instances of their parent class. The most common use of polymorphism is when a parent class reference is used to refer to a child class object. It enables a single function to operate in different ways based on the object that it is called on.

**Example in Kotlin:**
```kotlin
fun makeSound(animal: Animal) {
    animal

.sound()
}

val dog = Dog()
val cat = Cat()
makeSound(dog)  // Output: Bark
makeSound(cat)  // Output: Meow
```

---

## SOLID Principles

SOLID is a set of design principles that enable developers to write flexible, maintainable, and scalable software.

### 1. **Single Responsibility Principle (SRP)**
A class should have only one reason to change, meaning it should have only one responsibility.

**Example in Kotlin:**
```kotlin
class Report {
    fun generateReport() {
        // Logic for generating report
    }
}

class ReportPrinter {
    fun print(report: Report) {
        // Logic for printing report
    }
}
```

---

### 2. **Open/Closed Principle (OCP)**
Software entities (classes, modules, functions) should be open for extension but closed for modification. This means that the behavior of a module should be extendable without modifying its source code.

**Example in Kotlin:**
```kotlin
open class Shape {
    open fun area(): Double {
        return 0.0
    }
}

class Circle(val radius: Double) : Shape() {
    override fun area(): Double {
        return Math.PI * radius * radius
    }
}

class Square(val side: Double) : Shape() {
    override fun area(): Double {
        return side * side
    }
}
```

---

### 3. **Liskov Substitution Principle (LSP)**
Derived classes must be substitutable for their base classes. Objects of a derived class should be able to replace objects of the base class without affecting the correctness of the program.

**Example in Kotlin:**
```kotlin
open class Bird {
    open fun fly() {
        println("Bird is flying")
    }
}

class Sparrow : Bird() {
    override fun fly() {
        println("Sparrow is flying")
    }
}
```

---

### 4. **Interface Segregation Principle (ISP)**
Clients should not be forced to depend on interfaces they do not use. Instead of having one large interface, divide it into smaller, more specific ones.

**Example in Kotlin:**
```kotlin
interface Printer {
    fun print()
}

interface Scanner {
    fun scan()
}

class MultiFunctionPrinter : Printer, Scanner {
    override fun print() {
        println("Printing...")
    }

    override fun scan() {
        println("Scanning...")
    }
}
```

---

### 5. **Dependency Inversion Principle (DIP)**
High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g., interfaces or abstract classes), not on concrete implementations.

**Example in Kotlin:**
```kotlin
interface NotificationService {
    fun sendNotification(message: String)
}

class EmailNotification : NotificationService {
    override fun sendNotification(message: String) {
        println("Sending email notification: $message")
    }
}

class User(private val notificationService: NotificationService) {
    fun notifyUser(message: String) {
        notificationService.sendNotification(message)
    }
}

val emailNotification = EmailNotification()
val user = User(emailNotification)
user.notifyUser("Hello!")
```

---

## Conclusion
- Use **interfaces** to define a contract for multiple classes to follow. Interfaces are ideal for defining behavior that can be implemented differently by multiple classes.
- Use **abstract classes** to provide a base implementation and shared functionality. Abstract classes are better when you need to provide partial implementations or some shared code for subclasses.
- The four principles of **OOP**—**encapsulation**, **abstraction**, **inheritance**, and **polymorphism**—are essential for creating modular, reusable, and maintainable code.
- Following **SOLID principles** ensures that your software design is flexible, maintainable, and scalable.
```
