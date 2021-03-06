---
layout: post
title: Mixin/Module include - is not a composition
order: 90
---

Well known practice ancient Rome “Divide et impera” (Divide and rule). In programing we also try to break our code into smaller parts, compose and reuse them. In Ruby we can `include` and `extend` modules to add methods from another modules. But the pure fact that your code is DRY does not allow to call it a "Composition".
Yes now your Class/Object can respond to a new set of messages. But there dark side is - mixin creates implicit (hidden) pollution of Objects public interface. Do these methods really belong to this Class/Object? You should as yourself this question every time when you do a mixin (include a Concern in Rails).

Mixin makes sense when it uses internal state on an Object (operates with `@attributes` or methods of the Host object). But makes a little sense with static helpers that make no use of Object by itself, but use only input params.

Real Composition should be base on Dependency Injection and Separation of Concerns. Your methods should be grouped by explicit receiver (e.g not `obj.a_m1`, `obj.a_m2`, `obj.b_m1`, but `obj.a.m1`, `obj.a.m2`, `obj.b.m1`). And Composition in this case literally means not to mixin `A` and `B`, but to Inject instances of `A` and `B`, and assign them to `a` and `b` properties inside `obj`. In that way you dependencies are not hard encoded, and you can substitute them dynamically. But the main advantage of this approach is that `A` and `B` are self-contained and could be really reused.

Just think about these use-cases (on example of Rails app). Do they really make sense?

* path helpers mixed in Controller/View
* application helpers mixed in Controller/View

Another issue that is introduced by mixins is that mixed class makes to much assumptions about host class interface (expect that host class has some methods). Or even worth expects that host class has already mixed other classes, but these expectations are completely hidden (for example try to add `errors` capability from ActiveModel to PORO).