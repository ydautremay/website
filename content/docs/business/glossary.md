---
title: "Glossary"
type: "home"
zones:
    - "Business"
sections:
    - "BusinessIntroduction"
menu:
    BusinessIntroduction:
        weight: 40
---

Below you will find the definitions of terms related to business framework or the DDD approach.

### Anemic domain model

An anti-pattern where the domain objects have hardly any behavior. Frequently they are just data holders with
getter/setter access and the logic completely lives in services on top of the domain objects. This is contrary to the 
essence of object-oriented design which is to combine data and behavior together.  

This anti-pattern is really a procedural design in disguise. It has the cost of a good domain model but none of the benefits.

Read more [here](http://martinfowler.com/bliki/AnemicDomainModel.html).

### Bounded context

A bounded context is a pattern of the {{< term "strategic design" >}} aspect of the DDD which is about dealing with large
models and teams. Large models are divided into different bounded contexts and are being explicit about their 
interrelationships.

### CQRS

The Command Query Responsibility Segregation (CQRS) pattern is a notion that a different model can be used to update
information (command model) than the model used for reading information (query model). 

### Domain 

A sphere of knowledge, influence or activity. The subject area to which the user applies a program is the 
domain of the software. The domain is completely agnostic of the application logic.

### Domain-Driven Design

An approach to the development of complex software adding on OOP to allow designers to concentrate on Business concerns rather than infrastructure ones.

### Model

An abstraction (eg. class diagram, mind map, plain text) that describes a domain and can be used to solve 
problems related to that domain.

### Strategic design

TODO

### Tactic design

TODO

### Ubiquitous Language

Common, rigorous language build up between developers and domain experts. This language should be based on the Domain 
Model used in the software. It should evolve as the team's understanding of the domain grows.

### Value object

Objects holding related values and defined by them. They have no identity, meaning that two value objects holding the same 
values are equals. They should be immutable.