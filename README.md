# Android-Interview-Question-Core-Topics


## Root Class
- In Java, the root class is **Object**.
- In Kotlin, the root class is **Any**.

---

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
