# Bargaining Canisters

## Over time, Cycles will be able to do more computation

Services on the IC will be able to use [[Cycles]] to pay for services. However, over time Cycles will be able to "do more computation". This is a quick thought experiment into how canisters autonomously or manually resolve the cycle to computation disparity.


## What does this mean in practice?

Lets think about that this means from a Service Standpoint. 

Let's imagine we operate the `Foo Service`. Users can use our service to transform a `Foo` into a `Bar`. This code is fairly difficult to implement, but doable. We're go-getters and went and wrote the code.

The internet computer makes integrating services _very_ easy. Knowing this we decided to make our service accessible to the public. Our API is shaped as follows

```
actor {
	getFoo: shared (Foo) -> async Bar; 
}
```

If we deploy my canister as is, as users call our canister it drain the cycle balance, eventually run out, and be deleted! Ahh, All our money!


Thankfully, Cycles can be sent with calls. We can use the [Experimental Cycles](https://sdk.dfinity.org/docs/language-guide/cycles.html#_the_experimentalcycles_library) library for this.


We calculate that in **in 2021**:

- $1.00 -> 1 Cycles -> Creates 1 Bars
-> Each operation costs $1.

Creating Bars is expensive! 

We decide to ask that a customer sends 2 cycles per call.
1 covers our cost and 1 profit. 

For the sake of continuing this exploration without come wild story lets say that in in **2025** we know:

- $1.00 -> 1 Cycles -> Serving 10 Foos
-> Computation 10X'd! 

Our customers will likely be calling our `FooService` 1MM times a month. 

in **2021**
-> 1MM Calls -> COST 1MM Cycles, $1MM -> PROFIT 1MM Cycles -> $1MM

in **2025**
-> 1MM Calls -> COST 100,000 Cycles, $1MM -> PROFIT 1.9MM Cycles -> $1.9MM

That's a hefty bit of additional profit.. Users might not stand for `FooService` charging so much now that computational capacity has increased so much. At this point someone might come along and create a `FairFooService` that replaces ours scalping the difference in price. How can we keep our service competitive as computation on the IC increases?

## A First Approach

The most simple approach is we stash how much a user is expected to send in a variable. Over time, as computation rises, we decrease this number. At the time of writing, the `Experimental Cycles` library exposes a function `ExperimentalCycles::accept(Nat)`. Meaning, if a users sends us 2MM cycles, we can accept up-to N, and the rest will be **refunded to the caller**. 

Perfect right? Well, yes and no. Imagine now that as time has past. Some of the users of our FooService have seen their user basis explode! We have one customer `TooMuchFoos` who are calling 10MM times a month... They're our most important customer. Recall, we initially set our Margins fairly steep. `TooMuchFoos` is likely large enough where they might implement their own `FooService` unless we can offer them a better deal.

## Usage Based Pricing

TBD...









