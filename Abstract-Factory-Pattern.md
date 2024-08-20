# Abstract Factory Pattern in C++

## Overview

The Abstract Factory Pattern is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. This pattern is useful when you need to ensure that a set of related objects is used together.

**Key Points:**
- **Abstract Factory Pattern**: Creates families of related objects.
- **Factory Method Pattern**: Creates a single object but defers instantiation to subclasses.

## Why Use Abstract Factory?

When you have multiple related objects (like buttons, checkboxes, etc.) that belong to different families (e.g., Windows, Mac), and you want to ensure that you use the objects from the same family together, the Abstract Factory Pattern is a good choice. This pattern helps to maintain consistency among products.

## Example Scenario

Imagine you're designing a user interface (UI) toolkit that should work on different platforms like Windows and Mac. You want to create platform-specific UI elements like buttons and checkboxes, but you want to ensure that a Windows button is used with a Windows checkbox, and similarly for Mac.

## Components of Abstract Factory Pattern

1. **Abstract Product**: Declares an interface for a type of product object. (e.g., `Button`, `Checkbox`)
2. **Concrete Product**: Implements the abstract product interface. (e.g., `WindowsButton`, `MacButton`, `WindowsCheckbox`, `MacCheckbox`)
3. **Abstract Factory**: Declares an interface for creating abstract products. (e.g., `GUIFactory`)
4. **Concrete Factory**: Implements the operations to create concrete product objects. (e.g., `WindowsFactory`, `MacFactory`)

## Implementation

Here's a simple C++ implementation of the Abstract Factory Pattern:

```cpp
#include <iostream>

// Abstract Product: Button
class Button {
public:
    virtual void render() const = 0;
    virtual ~Button() {}
};

// Abstract Product: Checkbox
class Checkbox {
public:
    virtual void render() const = 0;
    virtual ~Checkbox() {}
};

// Concrete Product: Windows Button
class WindowsButton : public Button {
public:
    void render() const override {
        std::cout << "Rendering Windows Button." << std::endl;
    }
};

// Concrete Product: Mac Button
class MacButton : public Button {
public:
    void render() const override {
        std::cout << "Rendering Mac Button." << std::endl;
    }
};

// Concrete Product: Windows Checkbox
class WindowsCheckbox : public Checkbox {
public:
    void render() const override {
        std::cout << "Rendering Windows Checkbox." << std::endl;
    }
};

// Concrete Product: Mac Checkbox
class MacCheckbox : public Checkbox {
public:
    void render() const override {
        std::cout << "Rendering Mac Checkbox." << std::endl;
    }
};

// Abstract Factory
class GUIFactory {
public:
    virtual Button* createButton() const = 0;
    virtual Checkbox* createCheckbox() const = 0;
    virtual ~GUIFactory() {}
};

// Concrete Factory: Windows Factory
class WindowsFactory : public GUIFactory {
public:
    Button* createButton() const override {
        return new WindowsButton();
    }

    Checkbox* createCheckbox() const override {
        return new WindowsCheckbox();
    }
};

// Concrete Factory: Mac Factory
class MacFactory : public GUIFactory {
public:
    Button* createButton() const override {
        return new MacButton();
    }

    Checkbox* createCheckbox() const override {
        return new MacCheckbox();
    }
};

int main() {
    GUIFactory* factory;

    // Use a specific factory type
    factory = new WindowsFactory();
    Button* button = factory->createButton();
    Checkbox* checkbox = factory->createCheckbox();

    button->render();
    checkbox->render();

    delete button;
    delete checkbox;
    delete factory;  // Clean up to avoid memory leaks

    factory = new MacFactory();
    button = factory->createButton();
    checkbox = factory->createCheckbox();

    button->render();
    checkbox->render();

    delete button;
    delete checkbox;
    delete factory;  // Clean up to avoid memory leaks

    return 0;
}
```

## Key Differences from Factory Method Pattern

- **Abstract Factory**: Focuses on creating **families** of related products. It ensures that a set of related objects are used together (e.g., Windows button with Windows checkbox).
- **Factory Method**: Focuses on creating a **single product**. It delegates the instantiation of a product to subclasses.

If you find a class overloaded with multiple Factory Methods—each responsible for creating different types of related objects—this might indicate that the class is taking on too many responsibilities, which can violate the Single Responsibility Principle (SRP).

The Single Responsibility Principle states that a class should have only one reason to change, meaning it should have only one responsibility. When a class starts to handle multiple factory methods, each creating different related objects, it's easy for the class to become complex and hard to maintain. This is where the Abstract Factory Pattern can come to the rescue.

## When to Use Abstract Factory?

- When your system needs to be independent of how its objects are created.
- When you want to ensure that a family of related objects is used together.
- When you want to enforce consistency among products.

## Benefits of Using Abstract Factory

- **Separation of Concerns:** Each factory (like WindowsFactory and MacFactory) is responsible for creating a family of related products. This separation keeps the code cleaner and each class focused on a single responsibility.

- **Scalability:** If you need to add a new platform, say LinuxFactory, you can do so without modifying the existing factories. This aligns with the Open/Closed Principle (OCP), where classes should be open for extension but closed for modification.

- **Maintainability:** By avoiding a single class with multiple factory methods, the code becomes easier to maintain. Each factory class is simple and focused on a specific set of related products.

## Conclusion

The Abstract Factory Pattern is powerful when dealing with families of related objects, allowing for greater flexibility and consistency in object creation. By using this pattern, your code can be easily extended to support new families of products without modifying existing code.

For more information on other design patterns, check the corresponding README files in this directory.
