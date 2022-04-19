# Learning Domain-Driven Design

## Part 1: Strategic Design

### Chapter 1: Analyzing Business Domains

A company’s domain is its main area of activity

- They can have multiple domains

A subdomain is a fine-grain area of business activity

- McDonald's sells burgers, but it also must be good at real estate, have good finances, etc. 
- Each subdomain must work together to form a successful domain

There are three types of subdomains

- Core subdomain: This is what gives that company the competitive advantage

  - They are naturally complex and should be if they are your competitive advantage (if they were simple, then others would do it too)

  - They are not all technical. They can be outside of your program, you just have to understand what that is. 

- Generic Subdomain: This is what a company has in common with its competitors. 

  - A Jewelry Store’s online shopping site is a generic subdomain - every competitor also has one. The Jewelry itself is the core subdomain. 

- Supporting Subdomain: These subdomains support the company’s operations while providing no competitive advantage

  - Unlike the other two subdomains, supporting subdomains are very simple. 

  - Business logic is mostly resembling data entry-like functionality (CRUD/ETL)

Only core subdomains provide a competitive advantage. 

- Generic subdomains don’t provide an advantage because the same solution is likely being implemented by their competitors (Using google or oAuth for 2FA)
- Supporting subdomains don’t provide competitive advantages. They’re typically not even something the company is worried about if the competitors got a hold of. 

Complexity can be an effective measurement for identifying which type of subdomain something is. 

- Core vs. Supporting
  - How complex is the logic? Can you make any money off of that thing by itself? 
- Supporting vs. Generic? 
  - Would it be easier/cheaper to hack a solution up yourself or pay for an existing solution? 

Identifying subdomains is important because it allows you to choose appropriate tools/techniques to handle the complexities of the business requirements

Core subdomains change often, as they are a competitive advantage, so they are what most time is spent on improving, innovating, and optimizing. 

- Supporting subdomains do not change very often, as they are not usually worth the effort. 
- Generic subdomains change - but in the aspect of bug fixes, patches, etc. 

All subdomains are important, but the implementation of each can vary

- Core subdomains are your competitive advantage, therefore they must be developed in-house. 
  - Because of complexity, definitely need to have the best engineers on the job
  - Needs to be volatile and extensible 
- Generic subdomains are generally purchased or open-source solutions that can simply be implemented. 
- Supporting subdomains are inherently low complexity, lending them to be quickly developed, and even outsourced. 
  - No need to reserve your best engineers
  - No ready-made solution usually exists. 

It is important to identify subdomains and their types. It’s even more important to identify the boundaries of each subdomain.

- Can start by identifying the different subdomains by first separating the company into its departments - each being a broad subdomain of the company’s domain. 

Course-grained subdomains are not good enough to determine boundaries. 

- Break these coarse subdomains into finer-grained components 
  - Customer Service = Help Desk System + Phone System + Scheduling + Shift Management

When drilling down from coarse subdomains to finer subdomains, think of subdomains as a set of coherent use cases.

- When identifying finer subdomains, also consider the type of subdomain each is. 
- Keep drilling until drilling down further provides no more details than the last iteration
  - If all types of a subdomain are the same as the previous drill down
- Drilling down should be as thorough as possible for core subdomains. It can be less thorough for generic and supporting subdomains. 

Not all identified subdomains need to be noted as important

- You can filter out all subdomains that are not software related

Domain experts are subject matter experts on the company’s business needs

- Typically either the person coming up with requirements or the end-users. 
- Domain experts can be experts of the whole domain, or of a specific subdomain.

### Chapter 2: Discovering Domain Knowledge

Business problems exist at both the domain and the subdomain level. 

- Subdomains are finely-grained business problems whose goal is to improve a business area’s capabilities. 

We should not aim to be domain experts, but know how to communicate with them. 

- We should use the same terminology as them, both in conversation, but also in code. 
- System requirements are not the same as the domain behind those requirements. 

Whenever information is “translated” there is a loss of information. 

- Typically the domain experts talk with the system analysts, who then translate the system requirements into an analysis model. 
  - This model is then translated into action items/implementation by the developers

To avoid translations, DDD aims to use a ubiquitous language

- This is a language that everyone speaks
- The language and terminology the domain experts know their domain by. 
- Everyone involved, from shareholders to developers will speak in these terms. 

The ubiquitous language is that of the business. 

- NO Technical jargon

Consistency is the key to the ubiquitous language being successful

- Terms cannot mean two things - this causes ambiguity. 
  - This can be discerned in human interactions, but the software does not do well with ambiguity
- Multiple terms cannot reference the same thing (synonymous terms)
  - Use each term specifically within their context if there is a difference. 

Models are not a copy of the real thing, but a construct made to help make sense of the whole system

- Maps - each map does not have every detail of the earth, but just enough information for its specific purpose. 

All models have a purpose and models should only include enough details to fulfill that purpose. 

- Every model’s purpose is to solve a problem 
- All models are abstractions and abstractions are a tool used to handle complex things by omitting unnecessary details. 

When establishing a ubiquitous language, we are essentially creating a business model. 

- The model should reflect involved entities, their relationships with each other, behavior, and variants. 
- Establishing this language gets more important the more complex the business logic is.
  - One small misunderstanding can cause lots of bugs. 

Establishing the ubiquitous language should only be done with domain experts who can uncover inaccuracies or poor assumptions

- Developing the ubiquitous language should be an ongoing process, as the understanding of a domain gets deeper and deeper. 

You can use tools, such as wiki pages to help maintain the ubiquitous language used

- These tools do not replace the need to use the language

Developing a ubiquitous language requires asking a lot of questions

- There is some learning of what is already there, but also some co-creating to do with the domain expert when a process is not documented anywhere, and just known to someone. 

Implementing/developing a ubiquitous language when a language exists but does not reflect the domain effectively is hard.

- Requires patience. 
- Start by making sure the language is used where you can control it: Documentation and your codebase. 

### Chapter 3: Managing Domain Complexity

Sometimes coming up with a ubiquitous language to use can be very difficult because domain experts could use the same term to mean two different things

- “Lead” in sales means that someone is interested and you could reach out to make a sale, but in marketing, it could mean that someone has clicked on an advertisement link to the company’s site and is getting ready to make a purchase. 

Allowing this ambiguity is not a good idea, as when talking person to person, we can use our context clues to figure out which lead we’re referring to, but in software/ our source code, ambiguity quickly turns clean code into a mess. 

Solving this ambiguity in a domain-driven design is something called ==bounded context==.

- When ambiguity exists in terminology, one way to approach resolving this is by taking our domain and splitting it into two different smaller languages, or contexts. 
- The term “Lead” can now exist twice, but only once within each context (Sales context and Marketing Context)
  - Within each context, the terms mean two different things, and that’s perfectly fine. 

The problem a model is trying to solve is an intrinsic part of the model. That problem has boundaries to it - bounded contexts. 

- If it did not have boundaries, then the model would encompass too much, and then it would no longer be a model, but a copy of the real world

A ubiquitous language is not a *universal* language, and is not intended to be ubiquitous across an entire organization. 

- A ubiquitous language is ubiquitous within the scope of its bounded context.
- It is focused on describing only the model that is within this bounded context.

The size of a bounded context should be somewhat irrelevant - your goal is not to have a big or small context, your goal should be to have a useful one. 

- If you have a larger bounded context, and one portion of the context’s functionality will develop faster than the other portion, you can consider splitting it into smaller domains. 
- If the entirety of a context is a coherent piece of functionality, then it may not be a good idea to split the context up. 
  - In the future, if the coherent functionality is split up into smaller contexts, then they will have to be updated simultaneously any time changes are needed within this context. You can avoid this requirement by sticking with the larger context. 
- Redefining the size of your context is a large design effort and requires careful consideration

Subdomains vs. Bounded Contexts

- Subdomains are a breakdown of how the organization works and plans its strategy. 
  - These are not created by us, but instead, by the organization.
- Bounded contexts are designed - We decide how to divide the subdomains into smaller, more manageable problem domains. 

Theoretically, we could have one model covering the entirety of a domain. 

- If this model were too large, we could break it up into smaller bounded contexts to help make sense of the domain. 
  - If these contexts are still too large, we can break them up further.
- The size of our bounded contexts is a design decision. 

==Subdomains are discovered - bounded contexts are designed.==

The bounded context pattern is for defining physical and ownership boundaries

- Physical boundaries are created when defining bounded contexts because each context should be its own service/project. 
  - Each context should be able to be updated and modified independently of the rest. 
  - Clear physical boundaries between contexts allow us to implement each of them with technology stacks that are best suited for the problem. 
- Ownership boundaries are created when splitting a problem up into bounded contexts by allowing only one team to work on a bounded context.
  - Creating these ownership boundaries, it removes the possibility of implicit assumptions about a team might make about models. 
  - One team should carry out the implementation, evolution, and maintenance of a bounded context. 
  - Each bounded context should be owned by one team only, but one team can own multiple bounded contexts. 







### Chapter 4: Integrating Bounded Contexts



## Part 2: Tactical Design



### Chapter 5: Implementing Simple Business Logic





### Chapter 6: Tackling Complex Business Logic





### Chapter 7: Modeling the Dimension of Time





### Chapter 8: Architectural Patterns





### Chapter 9: Communication Patterns





## Part 3: Applying Domain-Driven Design in Practice



### Chapter 10: Design Heuristics



### Chapter 11: Evolving Design Decisions



### Chapter 12: EventStorming



### Chapter 13: Domain-Driven Design in the Real World



## Part 4: Relationships to other Methodologies and Patterns



### Chapter 14: Microservices



### Chapter 15: Event-Driven Architecture



### Chapter 16: Data Mesh















