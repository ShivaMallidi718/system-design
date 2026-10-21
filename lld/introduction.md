# Introduction to LLD (Low Level Design)

## What is it?
LLD is the detailed design of one part of a system, at the code level: which classes exist, what each is responsible for, and how they interact. Think of designing a single house in detail: rooms, doors, wiring. It is different from planning the whole city (HLD).

**Core question:** "How do I structure the code for X so it is clean, easy to change, and easy to extend?"

## What you actually decide
- Which classes, interfaces, and enums exist (`ParkingLot`, `Vehicle`, `Ticket`, ...)
- What each class is responsible for (and what it is not)
- Relationships: inheritance, composition, association
- Which design patterns fit (Strategy, Factory, Observer, ...)
- How to keep it thread-safe when many things happen at once
- How errors are handled

## Why it exists (the problem it solves)
Code that "just works" often becomes painful:
- Changing one thing breaks five others
- Adding a new feature means editing many files
- Classes do too many jobs and are impossible to test
- Duplicate logic is everywhere

Good LLD makes code **readable, extensible, testable, and maintainable**.

## Tools of LLD
1. **OOP basics:** encapsulation, abstraction, inheritance, polymorphism. Prefer composition over inheritance.
2. **SOLID principles:**
   - **S**ingle Responsibility: one class, one job
   - **O**pen/Closed: open to extension, closed to modification
   - **L**iskov Substitution: subclasses must be usable in place of parents
   - **I**nterface Segregation: small, focused interfaces
   - **D**ependency Inversion: depend on abstractions, not concrete classes
3. **Design patterns:**
   - Creational: Singleton, Factory, Builder, Abstract Factory, Prototype
   - Structural: Adapter, Decorator, Facade, Composite, Proxy, Bridge, Flyweight
   - Behavioural: Strategy, Observer, Command, State, Template Method, Chain of Responsibility, Iterator, and others
4. **UML class diagrams:** a visual way to show classes and relationships
5. **Concurrency:** threads, locks, thread safety, deadlocks, producer-consumer

## Simple example: Parking Lot
1. **Clarify:** vehicle types? multiple floors? payment? entry/exit gates?
2. **Find entities:** `ParkingLot`, `Floor`, `Spot`, `Vehicle`, `Ticket`, `Payment`
3. **Assign responsibilities:** `Spot` knows if it's free; `Ticket` records entry time
4. **Pick patterns:** Strategy for pricing, Factory for creating vehicle types
5. **Check:** can I add a new vehicle type without changing existing code? (Open/Closed)
6. **Write the code and test it**

## What an LLD interview looks like (about 45-60 minutes)
1. Clarify requirements and scope
2. Identify main classes and relationships
3. Draw a class diagram
4. Write clean code for core flows (often in your chosen language)
5. Discuss extensibility, patterns used, and edge cases

You are judged on clean structure, sensible patterns (not over-engineering), and how easily your design handles a new requirement.

## How this folder is organized
- `concepts/`: SOLID, design patterns, concurrency, UML (one note each)
- `designs/`: one note per problem (parking lot, vending machine, ...)
- `diagrams/`: your class diagrams

## Study order
1. OOP and SOLID
2. UML class diagrams
3. Design patterns (creational, structural, behavioural)
4. Concurrency basics
5. Practice problems (parking lot, logging framework, traffic signal, vending machine, task manager, then elevator, BookMyShow, Splitwise, ATM, chess, LRU cache)

Your playlist is missing Decorator and Observer. Add them yourself.

## Golden rules
- Don't force a pattern. Use one only when it solves a real problem.
- Start with simple classes, then refactor as requirements grow.
- Design it yourself first, then compare with the video.
- Pick one language and write all your LLD code in it.
