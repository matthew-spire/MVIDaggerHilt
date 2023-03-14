# Getting Started with Hilt (Dagger 2)

## Introduction
- Hilt is a tool built on top of Dagger 2 for dependency injection (DI)
- Get started setting up Hilt in Android Studio
  - Review documentation
    - [Hilt](https://dagger.dev/hilt/) documentation
    - [DI Android Developer](https://developer.android.com/training/dependency-injection) documentation
  - Build a project
  - Beginner to advanced

## What is Dependency Injection (DI)?
- Classes often require references to other classes
  - [Classes (OOP)](https://brilliant.org/wiki/classes-oop/#:~:text=A%20class%20is%20a%20way,Ferrari%2C%20and%20a%20Ford%20instance)
  - E.g., a `Car` class might need a reference to an `Engine` class
  - Required classes are called dependencies
    - E.g., the `Car` class is dependent on having an instance of the `Engine` class to run
- Three ways for a class to get an object it needs:
  - Class constructs the dependency it needs
    - E.g., `Car` would create and initialize its own instance of `Engine`
  - Grab it from somwehere else
    - Some Android APIs, such as `Context` getters and `getSystemService()`, work this way
  - **DI**/Have it supplied as a parameter
    - App can provide these dependencies when the class is constructed or pass them in to the functions that need each dependency
    - `Car` constructor would receive `Engine` as a parameter
- Without DI
  - ```
    class Car {
        private val engine = Engine()

        fun start() {
            engine.start()
        }
    }

    fun main(args: Array) {
        val car = Car()
        car.start()
    }
    ```
  - Why this is problematic:
    - ` Car` and `Engine` are tightly coupled
      - An instance of `Car` uses one type of `Engine` and no subclasses or alternative implementations can easily be used
      - If the `Car` were to construct its own `Engine`, then you would have to create two types of `Car` instead of just reusing the same `Car` for engines of type `Gas` and `Electric` &rarr; The code is not reusable
    - Hard dependency on `Engine` makes testing more difficult
      - `Car` uses a real instance of `Engine`, thus preventing the use of a test double to modify `Engine` for different test cases
- With DI
  - ```
    class Car(private val engine: Engine) {
        fun start() {
            engine.start()
        }
    }

    fun main(args: Array) {
        val engine = Engine()
        val car = Car(engine)
        car.start()
    }
    ```
    - `main` function uses `Car`
    - `Car` depends on `Engine` &rarr; App creates an instance of `Engine` and then uses it to construct an isntance of `Car`
  - Benefits of DI-based approach:
    - Reusability of `Car`
      - Can pass in different implementations of `Engine` to `Car`
      - E.g., if you define a new subclass of `Engine` called `ElectricEngine` that you want `Car` to use &rarr; If you use DI, then all you need to do is pass in an instance of the updated `ElectricEngine` subclass and `Car` still works without any further changes
    - Easy testing of `Car`
      - Can pass in test doubles to test different scenarios
      - E.g., create a test double of `Engine` called `FakeEngine` and configure it for different tests
- Advantages of implementing DI:
  - Reusability of code
  - Ease of refactoring
  - Ease of testing

## Hilt
- What is Hilt?
  - Provides a standard way to incorporate Dagger dependency injection into an Android application
- Goals of Hilt:
  - Simplify Dagger-related infrastructure for Android apps
  - Create a standard set of components and scopes to ease setup, readability/understanding, and code sharing between apps
  - Provide an easy way to provision different bindings to various build types (e.g. testing, debug, or release)

## Getting Started in Android Studio
- Create a new project
  - New Project > Phone and Tablet > Empty Activity and click "Next"
  - Give the project a name, e.g. "MVIDaggerHilt", and click "Finish"
- In the "Project" view
  - Open the project's root `build.gradle` file and add `id 'com.google.dagger.hilt.android' version '2.44' apply false` to the plugins
  - Open the `app/build.gradle` file and add
    - `id 'kotlin-kapt'` and `id 'com.google.dagger.hilt.android'` to the plugins
    - `implementation 'com.google.dagger:hilt-android:2.44'` and `kapt 'com.google.dagger:hilt-android-compiler:2.44'` to the dependencies
    - ```
      // Allow references to generated code
      kapt {
        correctErrorTypes = true
      }
      ```
- Hilt 