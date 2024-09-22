Android: Navigation

### **Jetpack Navigation**

Jetpack Navigation is a part of Android Jetpack and provides a framework to handle in-app navigation. It simplifies managing fragment transitions, argument passing, and deep linking, making it easier to navigate between different screens of your app.

---

### **Key Features of Jetpack Navigation**:
1. **Navigation Graph**: A visual map of your app's navigation structure. It defines all navigation paths, including fragment transitions and actions.
2. **NavController**: The central class that manages app navigation within the `NavHostFragment`.
3. **NavHostFragment**: A container where navigation destinations (fragments, activities) are swapped in and out.
4. **Safe Args**: A Gradle plugin that generates typesafe classes for navigating and passing data between destinations.
5. **Deep Links**: Enables navigation to a specific screen using URLs or custom app actions, even if the app isn't currently running.
6. **Back Stack Handling**: Automatically manages the fragment back stack for you, making it easier to handle system back presses.

---

### **Core Components of Jetpack Navigation**:

#### **1. Navigation Graph**:
A navigation graph (`nav_graph.xml`) defines all navigation paths in your app and connects destinations via actions. You can create and manage this graph visually using Android Studio’s Navigation Editor.

Example:
```xml
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    app:startDestination="@id/homeFragment">

    <fragment
        android:id="@+id/homeFragment"
        android:name="com.example.app.HomeFragment"
        android:label="Home">
        <action
            android:id="@+id/action_homeFragment_to_detailsFragment"
            app:destination="@id/detailsFragment" />
    </fragment>

    <fragment
        android:id="@+id/detailsFragment"
        android:name="com.example.app.DetailsFragment"
        android:label="Details"/>
</navigation>
```

#### **2. NavController**:
The `NavController` is responsible for navigating between destinations defined in the navigation graph. You can retrieve it using:

```kotlin
val navController = findNavController(R.id.nav_host_fragment)
```

#### **3. NavHostFragment**:
This is a container that swaps out fragments as you navigate between different destinations. You define it in the layout:

```xml
<androidx.fragment.app.FragmentContainerView
    android:id="@+id/nav_host_fragment"
    android:name="androidx.navigation.fragment.NavHostFragment"
    app:navGraph="@navigation/nav_graph"
    app:defaultNavHost="true"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

#### **4. Safe Args**:
Safe Args helps you pass arguments between fragments in a type-safe manner. It generates classes for each fragment and ensures that the argument types are checked at compile time.

Add Safe Args to your `build.gradle`:

```groovy
apply plugin: "androidx.navigation.safeargs.kotlin"

dependencies {
    def nav_version = "2.7.0"
    implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
    implementation "androidx.navigation:navigation-ui-ktx:$nav_version"
}
```

Usage:
```kotlin
val action = HomeFragmentDirections.actionHomeFragmentToDetailsFragment(itemId)
navController.navigate(action)
```

#### **5. Deep Links**:
You can set up deep links to navigate directly to a destination from external apps or URLs. For example:

```xml
<deepLink
    app:uri="https://www.example.com/details/{itemId}" />
```

You can then handle the deep link in your activity by passing it to the `NavController`.

---

### **Jetpack Navigation Example**:

1. **Step 1: Define Navigation Graph** (`nav_graph.xml`):
```xml
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    app:startDestination="@id/homeFragment">

    <fragment
        android:id="@+id/homeFragment"
        android:name="com.example.app.HomeFragment">
        <action
            android:id="@+id/action_homeFragment_to_detailsFragment"
            app:destination="@id/detailsFragment" />
    </fragment>

    <fragment
        android:id="@+id/detailsFragment"
        android:name="com.example.app.DetailsFragment" />
</navigation>
```

2. **Step 2: Setup `NavHostFragment` in XML**:
```xml
<androidx.fragment.app.FragmentContainerView
    android:id="@+id/nav_host_fragment"
    android:name="androidx.navigation.fragment.NavHostFragment"
    app:navGraph="@navigation/nav_graph"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

3. **Step 3: Handle Navigation in Code**:
```kotlin
// Navigate from HomeFragment to DetailsFragment
val navController = findNavController(R.id.nav_host_fragment)
navController.navigate(R.id.action_homeFragment_to_detailsFragment)
```

4. **Step 4: Using Safe Args** (if arguments need to be passed):
```kotlin
// Define action with argument in nav_graph.xml
<action
    android:id="@+id/action_homeFragment_to_detailsFragment"
    app:destination="@id/detailsFragment">
    <argument
        android:name="itemId"
        app:argType="integer" />
</action>

// Navigate with Safe Args
val action = HomeFragmentDirections.actionHomeFragmentToDetailsFragment(itemId)
navController.navigate(action)
```

---

### **Common Interview Questions on Jetpack Navigation**:

#### **1. What is Jetpack Navigation and why should you use it?**

**Answer**:  
Jetpack Navigation is a framework provided by Android Jetpack for managing in-app navigation. It simplifies fragment transitions, back stack management, argument passing, and deep linking. It offers type-safe argument passing with Safe Args, visual representation of the app's navigation structure, and automatic back stack management, reducing the complexity of manually handling fragment transactions.

---

#### **2. How does Jetpack Navigation handle the back stack?**

**Answer**:  
Jetpack Navigation automatically manages the back stack for you. It adds each destination to the back stack when you navigate between fragments, and it handles back presses by popping the fragments off the stack. You don’t need to manually manage `FragmentTransaction` or call `addToBackStack()` as Jetpack Navigation does it internally.

---

#### **3. What is `NavController` in Jetpack Navigation?**

**Answer**:  
`NavController` is the central component in Jetpack Navigation that handles app navigation. It manages fragment transitions, back stack operations, and deep linking. The `NavController` works in conjunction with the `NavHostFragment` to determine which fragment should be displayed at any given time.

---

#### **4. What are Safe Args in Jetpack Navigation?**

**Answer**:  
Safe Args is a Gradle plugin that generates type-safe classes to ensure argument passing between destinations (fragments) is type-safe and avoids runtime crashes due to incorrect argument types. It ensures that arguments passed during navigation are checked at compile-time, preventing common issues like `ClassCastException`.

---

#### **5. How do you define and handle deep links with Jetpack Navigation?**

**Answer**:  
Deep links in Jetpack Navigation are defined in the navigation graph. You can specify a URL or an intent action that should trigger a specific navigation action or destination. Jetpack Navigation handles deep linking even when the app is not running.

Example deep link definition in `nav_graph.xml`:
```xml
<deepLink
    app:uri="https://www.example.com/details/{itemId}" />
```

The deep link can be triggered from external sources, and the NavController navigates to the destination automatically.

---

#### **6. How do you handle navigation with `NavController` programmatically?**

**Answer**:  
You can navigate programmatically by retrieving the `NavController` and calling `navigate()` with the destination ID or action.

Example:
```kotlin
val navController = findNavController(R.id.nav_host_fragment)
navController.navigate(R.id.action_homeFragment_to_detailsFragment)
```

---

#### **7. What is the role of `NavHostFragment` in Jetpack Navigation?**

**Answer**:  
`NavHostFragment` is a container that swaps out fragments as you navigate between different destinations. It’s linked to the navigation graph and is responsible for displaying the current fragment on the screen.

---

#### **8. What are the advantages of using Jetpack Navigation over manual fragment transactions?**

**Answer**:  
- **Simplifies fragment transitions**: No need to manually handle `FragmentTransaction`.
- **Automatic back stack management**: Automatically handles fragment back stack and back presses.
- **Type-safe argument passing**: Safe Args ensures type-safe navigation between fragments.
- **Visual navigation graph**: Provides a clear visual representation of your app’s navigation.
- **Deep linking**: Simplifies deep linking within the app.
- **Ease of testing**: Makes testing navigation-related code easier.

---

#### **9. How do you handle fragment navigation with arguments using Safe Args?**

**Answer**:  
Using Safe Args, you can define arguments in the navigation graph and pass them using generated classes.

Example in `nav_graph.xml`:
```xml
<fragment
    android:id="@+id/detailsFragment">
    <argument
        android:name="itemId"
        app:argType="integer" />
</fragment>
```

Pass arguments programmatically:
```kotlin
val action = HomeFragmentDirections.actionHomeFragmentToDetailsFragment(itemId)
navController.navigate(action)
```

---

#### **10. How do you use Jetpack Navigation to implement navigation between activities?**

**Answer**:

  
You can use the `NavController` to navigate between activities by setting up `nav_graph.xml` for each activity, and then defining deep links or actions in the graph. You can trigger navigation programmatically using `navigate()` or use deep linking to move between different activities seamlessly.
