# Builder Design Pattern

## Overview

The Builder Design Pattern is a creational pattern used to construct complex objects step by step. Unlike other creational patterns that create objects in a single step, the Builder Pattern allows you to build an object by specifying its various parts or properties one at a time. This approach is particularly useful when an object has many attributes or when different variations of the object need to be created.

## When to Use the Builder Pattern?

- When the object to be created is complex and has multiple attributes.
- When you want to construct different representations of the same object.
- When you need to create an object step by step and ensure that all the necessary steps are performed.

#### Use the Builder pattern to get rid of a “telescoping constructor”.
Say you have a constructor with ten optional parameters. Calling such a beast is very inconvenient; therefore, you overload the constructor and create several shorter versions with fewer parameters. These constructors still refer to the main one, passing some default values into any omitted parameters.

```cpp
class Pizza {
    Pizza(int size) { ... }
    Pizza(int size, boolean cheese) { ... }
    Pizza(int size, boolean cheese, boolean pepperoni) { ... }
    //.....
};
```
The Builder pattern lets you build objects step by step, using only those steps that you need.

## Structure

The Builder Pattern involves the following key components:

1. **Builder (Interface)**: Declares the building steps that are common to all types of builders. Each step typically returns the builder itself, allowing for method chaining.
   
2. **Concrete Builder**: Implements the building steps defined by the Builder interface. This is where the actual construction of the product happens.
   
3. **Product**: The complex object that is being built. The Builder sets its parts or attributes.
   
4. **Director (Optional)**: An optional component that controls the construction process using a builder instance. It ensures that the construction steps are executed in a particular order.

## UML Diagram

```
Director ----> Builder (Interface)
                  ^
                  |
            ConcreteBuilder
                  |
                  |
               Product
```

## Example in C++

To fully understand the Builder Pattern, let's explore a more detailed example where we differentiate between basic and advanced features. We'll construct a `Car` object that can have both essential features (like engine and seats) and optional advanced features (like GPS, sunroof, and a high-end sound system).

### Problem: Complex Object Construction

Imagine you are designing a `Car` class where the car can have a variety of configurations. Some cars might just need the basics (engine, seats), while others might need more luxurious options (GPS, sunroof). Using traditional constructors would lead to a telescopic constructor issue where you’d have constructors with an increasingly large number of parameters, making the code hard to read and maintain.

### Solution: Builder Pattern

With the Builder Pattern, you can construct a `Car` step by step, making the process clear and flexible. We’ll separate the construction of basic features and optional advanced features to show how the pattern can simplify object creation.

### Step 1: Define the `Car` Product

First, define the `Car` class with the attributes that need to be set.

```cpp
#include <iostream>
#include <string>

class Car {
private:
    std::string engine;
    int seats;
    bool hasGPS;
    bool hasSunroof;
    std::string soundSystem;

public:
    void setEngine(const std::string& engine) { this->engine = engine; }
    void setSeats(int seats) { this->seats = seats; }
    void setGPS(bool hasGPS) { this->hasGPS = hasGPS; }
    void setSunroof(bool hasSunroof) { this->hasSunroof = hasSunroof; }
    void setSoundSystem(const std::string& soundSystem) { this->soundSystem = soundSystem; }

    void show() const {
        std::cout << "Car with " << engine << " engine, " << seats << " seats";
        if (hasGPS) std::cout << ", GPS";
        if (hasSunroof) std::cout << ", sunroof";
        if (!soundSystem.empty()) std::cout << ", and " << soundSystem << " sound system";
        std::cout << "." << std::endl;
    }
};
```

### Step 2: Define the Builder Interface

The `CarBuilder` interface declares methods for setting the car's attributes. We separate basic and advanced features.

```cpp
class CarBuilder {
public:
    virtual void buildEngine() = 0;
    virtual void buildSeats() = 0;
    virtual void buildGPS() = 0;
    virtual void buildSunroof() = 0;
    virtual void buildSoundSystem() = 0;
    virtual Car* getCar() = 0;
    virtual ~CarBuilder() {}
};
```

### Step 3: Implement the Concrete Builder

We'll create a concrete builder (`LuxuryCarBuilder`) that implements the `CarBuilder` interface. This builder can construct a basic car or add advanced features as needed.

```cpp
class LuxuryCarBuilder : public CarBuilder {
private:
    Car* car;

public:
    LuxuryCarBuilder() { car = new Car(); }

    void buildEngine() override { car->setEngine("V8 Engine"); }
    void buildSeats() override { car->setSeats(4); }

    // Optional Features
    void buildGPS() override { car->setGPS(true); }
    void buildSunroof() override { car->setSunroof(true); }
    void buildSoundSystem() override { car->setSoundSystem("Bose Surround Sound"); }

    Car* getCar() override { return car; }
};
```

### Step 4: Use a Director (Optional)

The `Director` class controls the construction process and can enforce specific sequences of building steps.

```cpp
class CarDirector {
private:
    CarBuilder* builder;

public:
    void setBuilder(CarBuilder* builder) { this->builder = builder; }

    // Builds a basic car with just engine and seats
    Car* buildBasicCar() {
        builder->buildEngine();
        builder->buildSeats();
        return builder->getCar();
    }

    // Builds a full-featured luxury car
    Car* buildLuxuryCar() {
        builder->buildEngine();
        builder->buildSeats();
        builder->buildGPS();
        builder->buildSunroof();
        builder->buildSoundSystem();
        return builder->getCar();
    }
};
```

### Step 5: Client Code

Finally, let's use the `LuxuryCarBuilder` and `CarDirector` to create different configurations of the `Car`.

```cpp
int main() {
    CarDirector director;
    LuxuryCarBuilder builder;

    director.setBuilder(&builder);

    // Build a basic car
    Car* basicCar = director.buildBasicCar();
    basicCar->show();
    delete basicCar;

    // Build a luxury car
    Car* luxuryCar = director.buildLuxuryCar();
    luxuryCar->show();
    delete luxuryCar;

    return 0;
}
```

### How It Works

- **Basic Features**: The `buildEngine` and `buildSeats` methods are called to create the basic version of the `Car`.
- **Optional Features**: The `buildGPS`, `buildSunroof`, and `buildSoundSystem` methods add advanced features to the car. These methods can be skipped if you only need a basic version of the car.
- **Director**: The `CarDirector` simplifies the client code by encapsulating the construction logic, ensuring that the car is built in a valid and consistent manner.

## Benefits of the Builder Pattern

- **Handles Telescopic Constructors**: The Builder Pattern avoids the problem of telescopic constructors by allowing you to add only the necessary parameters during construction.
- **Clear Separation**: There's a clear separation between the construction of basic and advanced features, making the code easier to understand and maintain.
- **Flexible Object Creation**: The same builder can be used to create different configurations of the product, like a basic or luxury car.
- **Improves Readability**: The step-by-step construction process is clear and easy to follow.
- **Flexible Object Creation**: You can create different variations of the object using the same construction process.
- **Encapsulation**: The construction details are hidden from the client, promoting encapsulation.

## Conclusion

The Builder Pattern is an excellent solution when you need to construct complex objects with a mix of required and optional features. It keeps the code organized, makes object construction flexible, and eliminates the issues caused by telescopic constructors.
