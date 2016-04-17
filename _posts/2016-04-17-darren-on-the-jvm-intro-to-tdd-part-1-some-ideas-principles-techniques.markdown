---
published: false
title: Darren on the JVM: Intro to TDD Part 1: Some Ideas, Principles & Techniques
layout: post
tags: [Agile, Java, Scala, JVM, Mocking, TDD]
---
> ### PROLOGUE
> 
> A while ago now I applied for a Java role at a certain newspaper,
> having heard they were Agile and proponents of TDD. After a successful
> initial telephone screening interview, I was issued a programming
> exercise - Battleships!  These exercises serve to reveal a candidates ability to
> 
> - Digest a problem description
> - Extract requirements and define scope
> - Identify expected program behaviour
> - but above all else... WRITE CODE!
> 
> In my case, I was given the opportunity to demonstrate my aptitude for TDD, as their required approach to every-day programming
> 
> ***SPOILER: I did not get the job <a href="javascript:a1.play()">:(</a>***
> 
> There is a some personal irony in this. Also I was unfortunately not given any constructive feedback either.
> TDD to some maybe old-hat or second-nature, but it is pervasive and still very much a component of many interview processes - which many, many candidates still fail - reflecting it is still a concern for hiring teams.
>
>In this short series of posts hopefully I can help those newcomers ramp-up and secure those much desired jobs and to perhaps offer those existing TDD'ers and indeed TDD-interviewers some added perspective.
<audio id="a1" src="http://www.soundjay.com/misc/sounds/fail-trombone-01.mp3"></audio>

In this blog I will demonstrate how I do Test Driven Development, with an approach - I call it 'The Introspective Approach' - that helps me get going when I get stuck.

In subsequent posts in the series I will use a variant of the Battleships game, run from the command-line, as the context for this learning exercise.
Battleships provides a good context as the game is quite familiar and, from a programming point of view, the inputs are easy to define and outputs easy to capture.

The complete exercise spec, along with the solution code will be available on GitHub; the code excerpts wherever included will link back to the relevant file and version there.

## First the Fundamentals

Let's boil down what testing and programming in general is; I offer:
> A program is just a function over some data; Given some input, a program will deterministically produce some output.

I say 'deterministic' as computers in general are well-behaved and we have dominion over all inputs, so basically the same inputs will produce the same outputs, ALWAYS!
~... although, what we don't know may... segmentation fault, NPE, BSD :D #geek-humour~   

Testing is merely defining the behaviour of a program - by stating your expectations for its outputs given a set of inputs - and verifying this behaviour holds true. The task then is to state the minimal set of expectations representative of all realistic input-output pairs, in the context of the business-domain or production-environment.  

Thus programming is the task of implementing the most concise yet general solution, for which that expected behaviour is verified.  

## Some Informal Guidelines... Techniques within Techniques

When TDD'ing, I use a few techniques to keep me on track and help me decide what to do next; these techniques are:

* _Defer-and-Delegate_
* _Mock_
* _Expand-to-Contract_ (Precursor to Refactor)
* _Refactor_

The order isn't meant to be prescriptive, but if you have a go you'll see it emerges naturally; let's look at how these techniques are related and facilitate one another.  

#### Defer and Delegate

Typically a program has a single point of entry e.g. like the `public static void main(String[] args)` method, but this seldom is the best place to contain all the application logic or make all the decisions.  

> At the crux of it is the _Single Responsibility_ principle

This technique advocates breaking down the program input and thus breaking down the problem space. If you defer any decision making over any part of the program input as late as possible, ironically this helps you to progress with the solution.  

When delegating as well, you decompose the problem and decision making and push those responsibilities off to other components; you end up with a bunch of classes collaborating - some orchestrating, some decision making, some consensus building - to form the system.  

If you focus on testing one part of the system, it's one decision or responsibility, then you can fix the behaviour of its collaborators to how you need them, in order to support implementing that decision logic or fulfilling that responsibility.  

Of course, this 'fixing of behaviour' is mocking.  

#### Mock

For this exercise I've used [Mockito](http://mockito.org/) and [ScalaMock](http://scalamock.org), but in general a mocking library allows you to mock or stub at least the public API of a component.  

This advocates the Design by Interface technique, which in turn supports/facilitates the _High Cohesion, Low Coupling_ and _Interface Segregation_ principles.  

If we delegate certain decision making to component dependencies, for each we define a public API with which to interact with that component dependency and that public API can be mocked.  

We know the part of the input that must be submitted to the component and we know what we need back - mocking allows us to install that behaviour for some or all of the duration of the test. Furthermore we can specify only that input will produce that particular output or we can fix it so that  the output we want is always produce, regardless of input.  

Mocking provides quite a lot of flexibility like that, but it's best not to get too carried away.
Generally it's not good to recursively mock mocks returning other mocks. That said, mocking factories - as you will see in the code - solves the problem of not being able to instantiate an interface, where they do not have implementations or constructors to call.  

#### Expand-to-Contract

A former boss once taught me this one; as an Agile coach and mentor, he observed me trying to go from no-code straight to the perfect-code. I have since observed many colleagues and clients do the same thing, that is:

1. Assume omniscience
1. Think ourselves super clever and don't need to go through the motions
1. By pure genius we can just materialise that perfect-code at the first attempt, BOOOM!

Nah! We are mere mortals - the compiler and tests will never let us forget this :D  

To get an understanding of what we are doing, we should first write code objectively (read: with focus), letting if-else and switch statements expand and for- and while-loops nest as deep as needed, duplicating freely to create a first-cut solution that works; this may seem counter intuitive and look a little silly, but hopefully it makes the problem space easier to understand and reveals whether you have covered all the bases.  

I want to say the trick here is to make duplication really big and obvious.  

Once it's working, THEN refactor! AND don't forget to revisit the nested loops and review their performance.  

#### Refactor

Refactoring is more an activity than a technique; there are a tonne of books on the topic and I won't delve too deep into it here. I'll just propose refactoring serves to simplify the code, make it more concise, readable, maintainable, debuggable - simple!  

One of the main techniques for refactoring is to hunt down and eliminate code duplication; this technique transcends paradigm boundaries (i.e. OO, to functional, to procedural, etc) and is particularly relevant for testing as...

> Every line of code written is a line of code that must be tested

In other words, make less work for yourself and reduce you maintenance and testing overhead.  

So those are the techniques; nothing really new, yet quite common to not see them done ;P
Also, if you've ever heard someone mention the term 'Design by Testing', these techniques are what they were talking about... or the outcome of these techniques, anyway.  

### The Introspective Approach

In my early years of practicing Agile and TDD specifically, I would often struggle to find a starting point.  

You may, for instance be given a grand vision for some system or a solution or get a bright idea, but some how you have to bridge the gap between that vision and your first TDD unit-test. As always when trying to achieve something, the first step is always the hardest! So I started using this Introspective Approach to overcome this inertia and get the TDD-ball rolling.  

Essentially you defer defining the system-, component-, class-, object- or subject-under-test and sort-of start in the _first-person_ as the test itself and let the implementation develop and emerge from there.  

You start with the highest level requirement and the highest level expectation i.e. the program input and the program output. Then define a single public method to consume that input and produce that output.

If designed well, any kind of test should be a black-box test, with which in this context public methods are the only means of interaction - this should always give us 100% coverage over any private methods

### Next Up

As mentioned at the start, the follow-ups to this post in the series are:

[Intro to TDD Part 2: Battleships, Java-style](#next-up)
:  A walkthrough of these techniques with concrete examples in Java

[Intro to TDD Part 3: Battleships, Scala-style](#next-up)
:  A walkthrough of these techniques with concrete examples in Scala.

Again, the context for both walk-throughs is the Battleships game, which will be specified by a few example scenarios each containing input data and expected output results.

As of this writing, the implementations for both are complete and the posts will (read: should) follow shortly.

Stay tuned!