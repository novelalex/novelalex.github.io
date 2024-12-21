+++
author = "Novel Alex"
title = "an attempt at a visual novel engine"
date = "2024-12-11"
draft = true
description = "a story about making a story making thing"
tags = [
    "gamedev", "programming"
]
+++

\[WIP]

A visual novel is a type of game, where you read a lot of text, and (optionally) interact with things. 

There are also visuals.

There are supposed to be visuals.

More on that later... 

I wanted to create a way for non-programmers to create visual novels by writing a simple script, with a syntax that can be read by anyone. 

I needed a Domain Specific Language or DSL for short, a DSL is a special kind of language that is kind of like a regular programming language but its made to do a specific task. 

For example, Python is a general purpose programming language. It can be used to build almost any software you could think of. On the other hand, Unreal blueprints are a good example of a DSL. It was designed to build logic for video games made inside of Unreal Engine.

I was considering my options, I could write a custom interpreter, or I could try to use Jetbrains MPS as a platform to create my DSL, but I had another option in mind.

At the time, I was messing around with a language called Ruby, which was flexible enough for me to define my own syntax. Ruby is well a well known language that is used widely... for web development. Not a lot of folks are using Ruby as an embedded scripting language for games, because although Ruby is a wonderful language to work in, it's not the fastest.

But I was in a unique situation, because unlike most game engines that need their scripts to run at least once every 16 milliseconds, I only needed the script to be run once(kinda) at the beginning of the program.

I was ready to use Ruby and C++ to build the engine, before making a graphical application using SDL, I decided to get a text based prototype working in the terminal.

```cpp
#include <mruby.h>   
```

The DSL is using a feature of Ruby called blocks. Blocks are pieces of code that can be given to a function as input, they can be stored for later use. Ruby's syntax for these functions mimics some features of the language and makes it seem as if they were part of the language.

This is what the syntax for writing stories looks like:

```ruby
character :wizard, "Wizzo the Wizard", :blue
character :tarnished, "Tarnished", :green

scene :start do
  description "You are at a crossroad in a forest."
  dialogue :tarnished, "Which path should I take?"
  choice "Take the left path", :left_path
  choice "Take the right path", :right_path
  choice "Take the cookie path", :cookie_path
end
```

A scene holds a timeline, which is just an array. The timeline contains each description or dialog that appears in order from top to bottom. 

The timeline for the `:start` scene would look like:
```ruby
[
    "You are at a crossroad in a forest.",
     {
        character: :tarnished, 
        text: "Which path should I take?"
    }
]
```

