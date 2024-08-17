# Factory Design Pattern

## Introduction

The Factory Design Pattern is a **creational design pattern** that provides a way to create objects without directly specifying their exact class. Instead, you use a factory that knows how to create different types of objects. It’s useful when you have a common interface or base class and you want to delegate the responsibility of instantiating concrete classes to a factory.


## Key Concepts

- **Factory Method**: A method in the factory class that decides which concrete class to instantiate based on input parameters.
- **Concrete Product**: The specific implementation of the product that the factory creates.
- **Product Interface**: A common interface or abstract class that all products share.

## When to Use the Factory Pattern

- When you want to centralize the creation of objects to ensure consistency and control.
- When you have a common interface or base class, and the exact type of the object isn't known until runtime.
- When the creation process involves complex logic that should not be exposed to the client.

## Example in C++

### Code Overview

Below is an example of a simple factory that creates different types of `Shape` objects like `Circle`, `Rectangle`, and `Triangles`.

```cpp
#include <iostream>

// Interface and should only contain all the pure virtual functions.
class Shape {
public:
    virtual void draw() const = 0;
    virtual ~Shape() {}
};

// Concrete Product: Circle
class Circle : public Shape {
public:
    void draw() const override {
        std::cout << "Drawing a Circle" << std::endl;
    }
};

// Concrete Product: Rectangle
class Rectangle : public Shape {
public:
    void draw() const override {
        std::cout << "Drawing a Rectangle" << std::endl;
    }
};

// Concrete Product: Triangle
class Triangle : public Shape {
public:
    void draw() const override {
        std::cout << "Drawing a Triangle" << std::endl;
    }
};

// Factory Class. Note that this class can provide some default implementation of the factory method too. All functions need not be pure virtual functions.
class ShapeFactory {
public:
    enum ShapeType {
        CIRCLE,
        RECTANGLE,
        TRIANGLE
    };

    static Shape* createShape(ShapeType type) {
        switch (type) {
            case CIRCLE:
                return new Circle();
            case RECTANGLE:
                return new Rectangle();
            case TRIANGLE:
                return new Triangle();
            default:
                return nullptr;
        }
    }
};

int main() {
    ShapeFactory::ShapeType shapeType;

    // Create and use a Circle
    shapeType = ShapeFactory::CIRCLE;
    Shape* shape1 = ShapeFactory::createShape(shapeType);
    shape1->draw();
    delete shape1;

    // Create and use a Rectangle
    shapeType = ShapeFactory::RECTANGLE;
    Shape* shape2 = ShapeFactory::createShape(shapeType);
    shape2->draw();
    delete shape2;

    // Create and use a Triangle
    shapeType = ShapeFactory::TRIANGLE;
    Shape* shape3 = ShapeFactory::createShape(shapeType);
    shape3->draw();
    delete shape3;

    return 0;
}
```

### Explanation

- **Product Interface**: The `Shape` class defines the interface for all shape objects.
- **Concrete Products**: `Circle`, `Rectangle`, and `Triangle` implement the `Shape` interface and provide specific behaviors for each shape.
- **Factory Class**: The `ShapeFactory` class contains a method `createShape` that takes a `ShapeType` enum and returns a new instance of the corresponding shape.
- **Client Code**: The `main` function shows how to use the factory to create different shapes without knowing the details of how they are created.

## Benefits of the Factory Pattern

- **Encapsulation**: The object creation logic is centralized in the factory, making the code easier to manage and modify.
- **Flexibility**: New product types can be added with minimal changes to the client code.
- **Decoupling**: The client code is decoupled from the concrete classes it uses, depending only on the product interface.

## Conclusion

The Factory Design Pattern simplifies object creation by centralizing it within a factory. This pattern is ideal for scenarios where the exact type of object to create isn't known until runtime, or when the object creation involves complex logic.
