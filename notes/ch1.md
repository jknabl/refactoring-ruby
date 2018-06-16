# Chapter 1: First example

* A poorly designed system is hard to change. We refactor things to make them easy to change. 
* IF you are writing a program that you don't expect to change, then duplicate/bad code is fine (see: shameless green). If you expect the program to change, then duplicative code is a nightmare to change.

> When you find you have to add a feature to a program, and the program's code is not structured in aconvenient way to add the feature, first refactor the program to make it easy to add the feature, then add the feature.
  * This is the "first make the change easy, then make the easy change" adage

* Solid tests are the starting point for refactoring.
* Smaller pieces of code make things more manageable, they are easier to work with and move around.
* Incremental changes -- test at every tiny step.
* Good code communicates what it is doing clearly, and variable names are a key to clear code.
* In most cases a method should be on the object whose data it uses
* While refactoring focus on quality/clarity, NOT performance.
  * e.g. yes you can cache values in a loop using a temp, but it adds complexity and gives up clarity. Until you have profiled and know you need the performance gain, you shouldn't bust out the caching temp. You should write clear code.
* When you are refactoring and introduce a method call in place of e.g. a temp, you need to make sure the method call is idempotent (tests)
* Temporary variables encourage long, complex routines. They are also often not needed.
* If you have to depend on one of two things, depend on the thing that changes the least.
