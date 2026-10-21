# HLD vs LLD (Introduction)

**Analogy:** HLD is planning a city. LLD is designing a single house.

## HLD: High Level Design
The big picture of the whole system. You decide which major pieces exist and how they connect.

- **Question:** "How do I build something like Instagram so 100 million people can use it?"
- **Decide:**
  - Which servers, databases, caches, and queues to use
  - How traffic is split across many machines
  - Where data is stored, and what happens when something fails
- **Draw:** boxes and arrows (client -> load balancer -> servers -> database/cache)
- **Think about:** scale, speed, reliability, trade-offs
- **Example topics:** load balancers, caching, sharding, Kafka, CAP theorem

## LLD: Low Level Design
The inside of one component, at code level. You decide how the classes and objects are structured.

- **Question:** "How do I write the code for a parking lot so it's clean and easy to change?"
- **Decide:**
  - Which classes exist (`ParkingLot`, `Vehicle`, `Ticket`)
  - What each class is responsible for
  - How classes relate, and which design patterns to use
- **Draw:** class diagrams (UML)
- **Think about:** clean code, SOLID principles, extensibility, thread safety
- **Example topics:** SOLID, Factory, Strategy, Observer, multithreading

## Side by side

| | HLD | LLD |
|---|---|---|
| Focus | Whole system and infrastructure | One module, down to classes and methods |
| Level | Architecture | Code structure |
| Main question | "Will it scale and stay up?" | "Is the code clean and extensible?" |
| Output | Architecture diagram | Class diagram plus code |
| Example | Design WhatsApp for 1B users | Design the classes for a parking lot |

## How they connect
HLD decides the pieces. LLD designs what goes inside each piece.
Example: in an Uber HLD you might have a "Matching Service" box. LLD is how you'd write the classes inside that service.

## In interviews
- **HLD round:** draw an architecture and discuss scale, databases, and trade-offs. More of a conversation.
- **LLD round:** write classes and code for a smaller problem (parking lot, vending machine), usually in 45-60 minutes.

## My plan
- DSA is the main track (about 70% of time).
- LLD: follow the video playlist, then add Decorator, Observer, and extra practice problems.
- HLD: follow `hld/HLD-System-Design-Roadmap.md`.
