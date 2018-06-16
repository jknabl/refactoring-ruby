# Chapter 2: Principles in Refactoring

* Clean code is easier to change than complex and messy code. 
  * Even great programmers rarely write clean code the first time around, hence refactoring.

> **Refactoring**: A change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its observable behaviour. 

* Only changes that make code easier to understand/change are refactorings (e.g. performance optimizations are not refactorings.)
* Kent Beck 'two hats': when you're writing software, you're either changing existing code or adding new functionality. You should never be both changing existing code and adding new functionality at once.
  * While refactoring, you don't even add any tests. Rely on what's already there (unless there was no coverage in the first place.)

* Code loses its structure over time as many programmers add stuff. Refactoring is a way to help code re-gain a sane structure.
* When you are trying to bash a program into working you aren't thinking about the next programmer who will have to deal with your crappy code. Refactoring can help make the next programmer's life easier.
* Refactoring helps develop code more quickly ($$$)
  * Poor design slows you (and every other developer) down. Good design (achieved via refactoring) helps us move quickly.

* Do not 'set aside time' for refactoring. Refactoring is something you do all the time in little bursts.
* *Rule of three* can be helpful guideline for when to refactor: The first time you do something, you just do it; 2nd time, you wince at the duplication, but do it anyway; third time, you refactor.

* Refactoring when you add functionality
  * Refactoring code while you read it can help you to understand code you didn't write.
  * If the functionality you need to add would be easier to add if some other change was made in the code first, then refactor.

> A senior developer once joined a team I was leading, halfway through the project. When he joined he saw things he didn't agree with and suggested that we refactor the code toward a better domain model. Anxious to learn from the senior developer, I paired with him over the next few days while we made vrious changes to the domain model. Unfortunately, many of the changes that the senior developer suggested could not be implemented due to additional constraints inposed by required features. In the end, the code was refactored to be slightly better; however, the largest benefit was the deep understanding that the senior developer gained from the refactoring. From that point forward he delivered value at the level you would expect from a team member who had been on the project from day one. 
  * Refactoring as an aid to understanding.

* If you can't convince your manager that refactoring is valuable, then don't tell your manager and refactor anyway (Kent Beck)

> If I need to add a new function and the design does not suit the change, I find it's quicker to refactor first and then add the function. If I need to fix a bug, I need to understand how the software works and I find refactoring is the fastest way to do this. **A schedule-driven manager wants me to do things the fastest way I can; how I do it is my business. The fastest way is to refactor; therefore I refactor.**

* Most refactoring involves introducting indirection into a program. Indirection can make code slightly harder to follow to inexperienced coders, but:
  * Enables sharing of logic
  * Allows you to explain _intention_ and _implementation_ separately (method calls and class names explain intention... if you need the implementation, you can go look at it)
  * Enables isolation of change.
  * Enables encoding of conditional logic to be clearer than endless case/if statmenets.

* Changing interfaces
  * It is not a problem to change an interface if you have access to all the callers of the changed interface. e.g. if you rename a method, but you know ALL of the places where it is called, you can change every instance and move on. 
  * It is a problem when the interface is a **published interface**, i.e. it is relied on by external systems.
    * If a refactoring changes a published interface, you have to retain both the old interface and the new one (at least until users have had the chance to react to the change.)
      * Old interface can call the new interface.
      * Should have deprecation functionality in place for gradual phase-out.
* It can be costly to publish interfaces. So don't publish interfaces unless you really need to.

* A compromise between totally re-writing crappy software and refactoring it is to refactor the system into components with strong encapsulation, and then refactor the worst ones one at a time.
  * This is kinda what S. did with the componentization push.

> The most costly refactoring is refactoring for academic purposes. Refactoring for academic purposes is in direct conflict with delivering working software. In your career you will likely find many lines of code that you do not agree with; however, disagreeing with implementation is not a good enough reason to refactor code. If the code currently hinders your ability to deliver software (or will in the future), you can refactor, but changing code because you philosophically disagree is simply wrong.

> If you can't prove to the business and the project manager that a refactoring is worth doing, you might be refactoring for academic purposes.

* Refactoring vs. up-front design. Get a 'reasonable', not perfect, solution drawn out first. Code it up, inevitably it will be flawed. Then refactor to get at a better design.
  * Totally up-front design places way too much importance on getting the design completely correct. This is a problem because no up-front design is ever correct, especially for large systems.

* It is impossible to guess what kinds of changes will need to be made to a system in the future. Don't try to guess at them in an up-front design or during a refactoring. 

> With refactoring you approach the risks of change differently. You still think about potential changes; you still consider flexible solutions. But instead of implementing these flexible solutions, you ask yourself, "How difficult is it going to be to refactor a simple solution into the flexible solution?" If, as happens most of the time, the answer is "pretty easy," you just implement the simple solution.

## Refactoring and performance

* Common for people to reject refactoring on the basis that it makes code slower.
* Refactoring makes software slower, but also more instrumentable/profilable/tuneable. 
  * The way to fast software is to write the instrumentable/profilable/tuneable solution first. Then profile and tune for sufficient speed. 
* Changes that improve performance usually make the program harder to work with. This slows development. This is not worth it in the vast majority of situations, esp. enterprise-style web apps.

> The interesting thing about performance is that if you analyze most programs, you find that they waste most of their time in a small fraction of the code. If you optimize all the code equally, you end up with 90 percent of the optimizations wasted, because you are optimizing code that isn't run much. The time spent making the program fast, the time lost because of lack of clarity, is all wasted time.

* The correct way to go is to not pay attention to performance at first and just write clear code. Then profile and speed up the parts that are taking up 90% of resources.
* If you have a well-factored system (high isolation, low coupling, etc.) it is easier to e.g. make it thread-safe for performance gains.
