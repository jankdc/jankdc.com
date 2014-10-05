---
layout: post
title: Occam - An Introspection
---

## Overview

For my final year project, I have decided to create yet another general purpose programming language named **Occam**. The main focus for the language is to reduce as much cognitive load for the programmer whilst being performant as much as possible.

<!-- more -->

In this context, the cognitive load is the amount of information or assumptions that one has to juggle in their working memory, in order to proceed in writing correct code. The justification for this focus is that, in my own personal experience, productivity and motivation decreases in proportion to the amount of complexity, alongside the amount of failed attempts to convey one's intent and create convenience due to lack of meaningful help from tooling (e.g. cryptic error messages, complex build systems, lack of library/package managers).

Furthermore, as time goes on without looking at one's codebase, one has to build the mental model about the implementation again in order to make sense of things, further decreasing production. Unfortunately, I have not found a way to remove that part of the human condition, and it's inevitable that complexity will arise as we add more features, but I aim at least to reduce the burden as much as possible. I will experiment with any features, either new or stolen from other languages, to see if they align with the objective in mind.

## Cognitive Load

Every programmer has that feeling. Depending on your experience, it comes up when you have written your first 10,000 lines or your first million. The feeling of *friction*. A feeling that in order to write a new line, one must consider a lot of constraints, and if they were disobeyed, subtle bugs may arise and can range from the mildly harmless to the cheeky ones that crashes<a href="http://en.wikipedia.org/wiki/Mars_Climate_Orbiter" target="_blank"> Mars Probes</a> for a hobby.

{% highlight c++ %}
// global_variables_galore.cpp
// We have all discussed the evil of global variables
// to the point of exhaustion so I'll refrain from
// explaining, but the code speaks for itself.

#include <iostream>

int globA = 10;
int globB = 5;

int funcA(int num) {
  if (num > globA) {
    globA = 1;
    return 0;
  } else if (num < globB) {
    globB = 10;
    return 1;
  }
}

int funcB(int num) {
  if (num < globA) {
    globA = 10;
    return 0;
  } else if (num > globB) {
    globB = 1;
    return 1;
  }
}

int main(int argc, char* argv[]) {
  std::cout << funcA(2) << "\n";
  std::cout << funcB(5) << "\n";
  std::cout << funcA(funcB(5)) << "\n"; // value?

  return 0;
}
{% endhighlight %}

Okay, I know this is contrive, but it was deliberate. Give a person a few weeks, some inevitable crunch times and a lack of sleep and they will have made the same mistakes with a factor of 10. The point is, we are allowed to do this kind of mess even though there are better options. The amount of conditional checks we have to make for that small amount of code is ridiculous.

People are so prone to spaghetti code that we've invented different ways of abstraction (e.g. functions, OOP objects, threads) to encapsulate them into workable chunks. However, people just love to be clever without thinking of their future selves and others, and so we write code that violate the principles of those abstractions, causing dependencies between the contraints until we're back where we started. There needs to be a balance between the freedom to express and the amount of complexity that freedom could cause.

## Performance

The use case of the language will involve around systems that require deterministic performance such as high end video games and other systems that has time as it's main requirement. Therefore, it needs to be as fast possible whilst remaining predictable on how memory is managed. Traditional garbage collection (GC) as a memory model is not a good candidate for this one. On the other hand, manual memory management is not my cup of tea either. It's error prone and most of the time, follow the same pattern of allocation and deallocation in the same scope, which is redundant. I will have to think about this one in further detail. Although, I have seen a language where this has been done without traditional GC or manual memory management. Check <a href="https://forge.open-do.org/plugins/moinmoin/parasail/">this</a> out. The ParaSail language essentially frees the memory of objects when they go out of scope, which is an interesting idea. This could mean removing the idea of memory management and pointers in general from end users (e.g. programmers)! Although, I'd love someone to prove me wrong.

## Error Handling

The language should provide you with only one paradigm to report an error. Having more than one is unnecessary. As how handling an error goes, I'm still not sure. I like the idea of <a href="http://www.rust-lang.org/">Rust</a>'s error handling, but I feel like I'm trying too much to ask for permission in order to use the given resources. However, I think it just takes time to get used to, or at least needs to have better tooling to reduce that kind of friction.

## Conclusion (For Now)

So far, this is the best that I could come up with:

- <a href="http://en.wikipedia.org/wiki/Pure_function">Pure functions</a> are good! Make them the default.
- Impure functions still exists because we need IO. However, they must be explicit (what mechanism to use is still in the air).
- Global variables are bad! There are better ways to solve the problem they are trying to address, which is to reference common data.

All this brainstorming is error prone, as I am sure that I have many use cases that I've missed which could potentially dismiss any and all of my ideas. Hence, all of the things above are not set in stone (yet). The best way for me is to start small and get a 'feel' of how it 'flows' (much scientific) when I'm writing algorithms or code in general. In the next week, I will create an interpreter to test out my ideas and move on from there.