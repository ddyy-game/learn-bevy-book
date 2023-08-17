# Learn Bevy Book

## Introduction to this book

- Bevy is a game engine that provides first-class low-level mechanisms for game developing
- Instead of feature-full, Bevy is more like "mechanism-full"
- We may use these mechanisms to create our own features, strategies, fancy ideas, and eventually, _games_
- Building games from low-level mechanisms is overwhelming at first. We want to alleviate this initial pain.
- This guide takes a top-down approach to give you a creator's guidance to learning Bevy.
- Some questions I will try to answer:
  - How to start creating from scratch?
  - When creating something, what mechanisms can you use?
  - When you want to change something, what parts should you adjust?

## Brief introduction to Bevy

From the [offical website](https://bevyengine.org):

> Bevy is a refreshingly simple data-driven game engine built in Rust.

- Compared with other game engines, Bevy is representing and tracking the internal data more elegantly
  - it helps you materialize the imaginary game world as concrete data
  - it organizes data as Entity-Component-System (ECS), a time-tested paradigm seen in many other engines
  - ...yet it leverages advanced Rust features to make it refreshingly simple for us to use
- How is my experience working with Bevy?
  - Accurate: I fully control the code, the code fully controls the game world
  - Confident: less unexpected behaviors or mysterious bugs, thanks to Rust and Bevy's clear architecture
  - Creative: put things together creatively, or craft your own features, all as I want
  - Agile: I can build, publish and iterate your game real quick
