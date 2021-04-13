# Introduction
Tiers are physical (not code) logical separations of components in an application or a service.

# Single Tier
All components all reside in the same machine (e.g MS Office, Pc game etc).

### Advantages:
- no network latency;
- data is easily and quickly available;
- data safety, since it's not transmitted over network;

### Disavantages:
- no control after application is shipped (hard to bug fix, make improvents, etc);
- application is vulnerable to be tweaked and reversed engineered;
- application's performance heavily depends on the user's machine;

# Two Tier
Client and server. Client'd have the UI an business logic in one machine. Backend server'd have the DB, which is hosted and controlled by business.

> Why not have the businees logic in another tier?

A few use cases takes advantages of this architecture, like app based games and simple apps (to do list etc). Mainly, cases in which even if the code is accessed by third party, no harm is done to business.

### Advantages:
- less calls to backend server, so lower latency;
- lesser calls, less money to be spent on servers;

### Disavantages:
- basicaly the same as in a single tier;

# Three Tier
Very popular and largely used by simple websites. The UI, application logic and DB are in different machines.

### Advantages:
- business has more control over the application logic, easier to maintain and improve;
- application logic is more secure;
- doesn't rely on the user's machine to perform well;

### Disavantages:
- more network calls, more latency;
- more costs with servers;

# N Tier
More than 3 components separated physically in their own machines, also called _**distributed applications**_.

Reasons for this level of separation:
- Single Responsability Principle: each component should have a single responsability and execute it well;
- Separation of Concerns: each components responsability should overlap as minimal as possible with others';

### Layers !== Tiers
Layers represent the organization of the code, like the UI layer, business layers, service layers, data access layer etc. Tiers are physycal separation of components.