# Prototype Design Pattern

### Introduction

The Prototype design pattern is a **creational pattern** that allows you to create new objects by copying or cloning an existing object, known as the prototype. This pattern is useful when creating objects is resource-intensive, and you want to avoid the overhead of initializing new objects from scratch. Instead of creating ojbects from scratch, this pattern creates similar object by cloing existing ones. The clone perfroms a Deep Copy, hence any changes made to the clone oject doesn't change the value of original object. 

### When to Use the Prototype Pattern
- When object creation is costly (in terms of time or resources).
- When the system should be independent of how its objects are created, composed, and represented.
- When you need to create objects that are similar but not exactly the same.

### Structure of the Prototype Pattern
The pattern typically involves the following components:

1. **Prototype Interface:** Declares a cloning method.
2. **Concrete Prototype:** Implements the cloning method. This class defines the object that will be copied.
3. **Client:** Creates a new object by requesting a clone from the Prototype.

### Example in C++

Here's a simple example of how you might implement the Prototype pattern in C++:

```cpp
#include <iostream>
#include <unordered_map>

// Prototype Interface
class Shape {
public:
    virtual Shape* clone() const = 0;
    virtual void draw() const = 0;
    virtual void modify(int newValue1, int newValue2 = 0) = 0;
    virtual ~Shape() {}
};

// Concrete Prototype: Circle
class Circle : public Shape {
private:
    int radius;
public:
    Circle(int r) : radius(r) {}
    Shape* clone() const override {
        return new Circle(*this); // Deep copy of the existing object
    }
    void draw() const override {
        std::cout << "Drawing a Circle with radius: " << radius << std::endl;
    }
    void modify(int newRadius, int) override {
        radius = newRadius;
    }
};

// Concrete Prototype: Rectangle
class Rectangle : public Shape {
private:
    int width, height;
public:
    Rectangle(int w, int h) : width(w), height(h) {}
    Shape* clone() const override {
        return new Rectangle(*this); // Deep copy of the existing object
    }
    void draw() const override {
        std::cout << "Drawing a Rectangle with width: " << width 
                  << " and height: " << height << std::endl;
    }
    void modify(int newWidth, int newHeight) override {
        width = newWidth;
        height = newHeight;
    }
};

// Client
class PrototypeFactory {
private:
    std::unordered_map<std::string, Shape*> prototypes;
public:
    PrototypeFactory() {
        prototypes["Circle"] = new Circle(10);
        prototypes["Rectangle"] = new Rectangle(5, 7);
    }
    
    Shape* createShape(const std::string& type) {
        return prototypes[type]->clone();
    }

    ~PrototypeFactory() {
        // Delete all prototypes to avoid memory leaks
        for (auto& pair : prototypes) {
            delete pair.second;
        }
    }
};

int main() {
    PrototypeFactory factory;

    // Create a clone of the Circle and modify the original
    Shape* circle = factory.createShape("Circle");
    Shape* circleClone = circle->clone();

    std::cout << "Before modifying the original Circle:" << std::endl;
    circle->draw();
    circleClone->draw();

    // Modify the original Circle
    circle->modify(20);

    std::cout << "\nAfter modifying the original Circle:" << std::endl;
    circle->draw();
    circleClone->draw();

    // Create a clone of the Rectangle and modify the original
    Shape* rectangle = factory.createShape("Rectangle");
    Shape* rectangleClone = rectangle->clone();

    std::cout << "\nBefore modifying the original Rectangle:" << std::endl;
    rectangle->draw();
    rectangleClone->draw();

    // Modify the original Rectangle
    rectangle->modify(15, 10);

    std::cout << "\nAfter modifying the original Rectangle:" << std::endl;
    rectangle->draw();
    rectangleClone->draw();

    // Clean up manually to avoid memory leaks
    delete circle;
    delete circleClone;
    delete rectangle;
    delete rectangleClone;

    return 0;
}

```

### Explanation
- **Prototype Interface (`Shape`)**: This abstract class declares the `clone()` method that returns a `Shape*`, allowing objects to be cloned.
- **Concrete Prototypes (`Circle` and `Rectangle`)**: These classes implement the `clone()` method, enabling them to be copied.
- **Prototype Factory (`PrototypeFactory`)**: The factory stores a collection of prototype objects. The `createShape()` method clones a prototype and returns the copy to the client.
- **Main Function**: Demonstrates how to create new objects using the prototype pattern by cloning existing shapes.

### Key Points
- Cloning an object is often faster than creating a new one from scratch.
- The Prototype pattern allows for the flexibility to add or modify prototypes without changing the code that uses them.
- You can enhance the Prototype pattern by using deep or shallow copies, depending on the requirements.

This pattern is particularly useful when dealing with complex objects that need to be instantiated multiple times with similar configurations.
