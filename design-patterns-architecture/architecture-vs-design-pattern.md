# Architecture vs Design Pattern

Link tham khảo: [https://stackoverflow.com/a/4243407](https://stackoverflow.com/a/4243407)

**Patterns** are distilled commonality that you find in programs. It allows us to deconstruct a large complex structure and build using simple parts. It provides a general solution for a class of problems.

{% hint style="info" %}
A large complex software goes through a series of deconstruction at different levels. At large level, architectural patterns are the tools. At smaller level, design patterns are the tools and at implementation level, programming paradigms are the tools.
{% endhint %}

For a most simplistic view:

* **Programming paradigms** - specific to programming language
* **Design patterns** - solves reoccurring problems in software construction
* **Architectural patterns** - fundamental structural organization for software systems

**Design patterns** are usually associated with code level commonalities. It provides various schemes for refining and building smaller subsystems. It is usually influenced by programming language

While **architectural patterns** are seen as commonality at higher level than design patterns. Architectural patterns are high-level strategies that concerns large-scale components, the global properties and mechanisms of a system

{% hint style="info" %}
If you have followed the thoughts laid above. You will see that **Singleton** is a "design pattern" while **MVC** is one of the "architectural" pattern to deal with separation of concerns.
{% endhint %}
