---
layout: post
title: "Fear (less | full) programming"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Programming languages either do or do not mesh with the way one thinks. I have
come to realize that programming speed is not as much about knowing the vocabulary
as much as lessening the "fear factor".

A simple demonstration:

    function length_of_string(input) {
        a = input.len
        return a;
    }

This is a function that *should* work in some language. I did not think about which one.

So let me think about doing this in Python:

    def length_of_string(input):
        a = input.len
        return a

When I wrote the line `a = input.len`, I had a moment of anxiety. Should it by 
`input.len()`? or `input.__len__()`? or len(input)?

This second-guessing slows me down and starts casting doubts on what I'm doing.

Implementing, or starting with, fearlessness is an important aspect of how to write
code for me. Implementing a function should ensure that the caller does not need to
have fear. The function should handle most cases, and should responsibly handle
the request.

One way I've been realizing a solution is by going at it all from testing. Tests
should be EASY EASY EASY. For example

    class MyTest(unittests.TestCase):
        def test_first()
            resp = requests.get("/users")

        def test_unit():
            from some.helpers.file import length_of_string

            resp = length_of_string("some string")
            assertTrue(resp.is_digit)

The idea is that I should just be able to code, and not think. Like I can talk/converse
and not think.

So far python makes me have fear, as do most other languages. Maybe I'm just not proficient yet?
