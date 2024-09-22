Android: Dagger 2 and Hilt

### **Dagger 2 and Hilt in Android**

**Dagger 2** and **Hilt** are both dependency injection (DI) frameworks used in Android development. Dagger 2 has been widely used for many years, and **Hilt** is a new dependency injection framework built on top of Dagger 2, designed specifically for Android to simplify DI.

---

### **Dependency Injection (DI)**

Dependency Injection is a design pattern used in software development to reduce the coupling between classes and make code more testable, reusable, and maintainable. Instead of a class creating its own dependencies, these dependencies are provided to the class from outside. In Android, DI helps manage the dependencies of Activities, Fragments, ViewModels, etc.

---

### **Dagger 2 Overview**

**Dagger 2** is a fully static, compile-time dependency injection framework for Java and Android. It helps manage and provide dependencies in large-scale applications efficiently.

#### **Key Components of Dagger 2**:
1. **@Module**: Provides dependencies. It is a class annotated with `@Module` where you define `@Provides` methods that tell Dagger how to create a dependency.
2. **@Provides**: Inside a `@Module`, this annotation tells Dagger how to provide instances of a certain type.
3. **@Inject**: Marks a constructor or field to be injected by Dagger.
4. **@Component**: The bridge between the module that provides dependencies and the classes that require them. The `@Component` interface connects dependency providers and consumers.
5. **@Scope**: Defines a scope for the provided dependencies (e.g., Singleton, ActivityScope).
6. **@Qualifier**: Used to differentiate between different objects of the same type.

---

### **Hilt Overview**

**Hilt** is built on top of Dagger and is specifically tailored for Android development to simplify dependency injection. Hilt integrates deeply with Android components like `Activity`, `Fragment`, `ViewModel`, etc., to manage their dependencies automatically. It reduces boilerplate code and makes DI more approachable for Android developers.

#### **Key Components of Hilt**:
1. **@HiltAndroidApp**: Annotate the `Application` class to trigger Hilt's code generation. It creates a dependency container at the application level.
2. **@AndroidEntryPoint**: Annotate Android components (`Activity`, `Fragment`, `Service`, etc.) to automatically inject dependencies into these components.
3. **@Inject**: Used to request dependencies (in the constructor or field).
4. **@Module**: Same as Dagger’s `@Module`, but used in Hilt to define how to provide dependencies.
5. **@InstallIn**: Defines the scope (e.g., `SingletonComponent`, `ActivityComponent`, `ViewModelComponent`) of the module's bindings.
6. **@Provides**: Similar to Dagger’s `@Provides` but with simplified scoping and lifecycle awareness.
7. **@Singleton**: Defines the scope of a dependency. It tells Hilt to provide the same instance of the dependency across the entire app.

---

### **Comparison: Dagger 2 vs Hilt**

| Feature                     | **Dagger 2**                            | **Hilt**                                    |
|-----------------------------|-----------------------------------------|---------------------------------------------|
| Setup Complexity             | Requires manual setup of `Component` and Modules. | Simplifies setup with `@HiltAndroidApp`.    |
| Android Integration          | Requires custom integration with Android components. | Built-in integration with Android components. |
| Scopes                       | Custom scopes need to be defined for each use case. | Predefined scopes for common Android components. |
| Code Boilerplate             | Requires more boilerplate code.         | Reduces boilerplate significantly.          |
| Gradle Build Speed           | Slower due to Dagger's annotation processing. | Faster build times compared to Dagger 2.    |
| Lifecycle Awareness          | Requires manual management of lifecycle. | Automatic lifecycle awareness and scoping.  |

---

### **Setting up Dagger 2**

#### **Step 1: Add dependencies**
In the `build.gradle` file:
```groovy
implementation 'com.google.dagger:dagger:2.x'
annotationProcessor 'com.google.dagger:dagger-compiler:2.x'
```

#### **Step 2: Define a Module**

```kotlin
@Module
class NetworkModule {

    @Provides
    fun provideRetrofit(): Retrofit {
        return Retrofit.Builder()
            .baseUrl("https://api.example.com")
            .build()
    }
}
```

#### **Step 3: Create a Component**

```kotlin
@Component(modules = [NetworkModule::class])
interface AppComponent {
    fun inject(activity: MainActivity)
}
```

#### **Step 4: Inject Dependencies**

```kotlin
class MainActivity : AppCompatActivity() {
    
    @Inject lateinit var retrofit: Retrofit

    override fun onCreate(savedInstanceState: Bundle?) {
        DaggerAppComponent.create().inject(this)
        super.onCreate(savedInstanceState)
        // Now retrofit is injected and ready to use
    }
}
```

---

### **Setting up Hilt**

#### **Step 1: Add dependencies**
In the `build.gradle` file:
```groovy
// Hilt dependency
implementation "com.google.dagger:hilt-android:2.x"
kapt "com.google.dagger:hilt-android-compiler:2.x"

// For ViewModel Injection
implementation "androidx.hilt:hilt-lifecycle-viewmodel:1.x"
kapt "androidx.hilt:hilt-compiler:1.x"
```

#### **Step 2: Define `@HiltAndroidApp` in Application Class**

```kotlin
@HiltAndroidApp
class MyApplication : Application() {
    // Hilt will create and manage the app-level dependency container
}
```

#### **Step 3: Create a Module**

```kotlin
@Module
@InstallIn(SingletonComponent::class)  // Define scope
object NetworkModule {

    @Provides
    @Singleton
    fun provideRetrofit(): Retrofit {
        return Retrofit.Builder()
            .baseUrl("https://api.example.com")
            .build()
    }
}
```

#### **Step 4: Inject Dependencies**

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {

    @Inject lateinit var retrofit: Retrofit

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Now retrofit is injected and ready to use
    }
}
```

---

### **Advantages of Hilt over Dagger 2**:
1. **Less Boilerplate**: Hilt eliminates the need for manual component creation and reduces boilerplate, especially in large projects.
2. **Built-in Android Scopes**: Hilt automatically handles common Android scopes (e.g., Activity, Fragment, ViewModel).
3. **Simplified Setup**: With `@HiltAndroidApp`, `@AndroidEntryPoint`, and `@InstallIn`, Hilt simplifies the process of defining where and how dependencies are injected.
4. **Lifecycle Awareness**: Hilt integrates with Android’s lifecycle components like `ViewModel` and `WorkManager`, providing proper scoping and cleanup of dependencies.
5. **Faster Iteration**: Hilt generally results in faster build times compared to Dagger 2 due to its streamlined dependency injection setup.

---

### **Interview Questions on Dagger 2 and Hilt**

#### **1. What is Dependency Injection, and why is it important in Android?**
**Answer**: Dependency Injection (DI) is a design pattern where a class’s dependencies are provided from the outside rather than the class creating them itself. In Android, DI helps in improving testability, modularity, and maintenance by decoupling class dependencies.

---

#### **2. How does Dagger 2 differ from Hilt?**
**Answer**:
- **Dagger 2** requires manual setup of `Component` and modules, whereas **Hilt** simplifies the process by auto-generating components.
- **Hilt** is built on top of Dagger 2 but integrates deeply with Android components and handles Android-specific scopes like `Activity`, `Fragment`, and `ViewModel` more effectively.
- **Hilt** reduces boilerplate code and improves lifecycle management.

---

#### **3. What is the role of the `@Inject` annotation in Dagger 2 or Hilt?**
**Answer**: The `@Inject` annotation is used to request a dependency. It can be used to annotate constructors, fields, or methods. Dagger or Hilt will automatically inject the required dependencies into the class where `@Inject` is used.

---

#### **4. How does Hilt manage the lifecycle of dependencies?**
**Answer**: Hilt is lifecycle-aware and scopes dependencies based on the Android component they are injected into. For instance, dependencies injected into an `Activity` are scoped to the activity’s lifecycle, and those injected into a `ViewModel` are scoped to the ViewModel’s lifecycle.

---

#### **5. Can you explain the role of `@Module` and `@Provides` in Dagger and Hilt?**
**Answer**: `@Module` is used to define classes that provide dependencies, while `@Provides` is used inside a module to specify how to construct and provide a particular dependency. In Hilt, `@InstallIn` is also used to define the scope in which the dependencies should be available.

---

#### **6. How would you handle injecting a `ViewModel` using Hilt?**
**Answer**: Hilt provides a `@ViewModelInject` annotation (or `@HiltViewModel` in newer versions) to inject dependencies into `ViewModel`. You simply annotate the `ViewModel
