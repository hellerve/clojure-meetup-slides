Hi! I'm Veit. I work here at Bright and I don't do Clojure.
I am working on the backend and most of my work is in Python.

Why am I talking now, then? Well, as you can see on the slide behind me,
I wrote a Lisp (yawn) in Haskell. Of course, this is nothing new.
People have done it before, although, surprisingly, very few have actually
delivered a somewhat complete project.

Zepto is based on Scheme, but it has a few things in common with Clojure.
Miraculously, despite utter ignorance about Clojure, I managed to reach
a few conclusions that are somewhat similar to Rich's. Some of them,
admittedly, only after I watched him speak.

Zepto is somewhat compatible with Scheme. Most things will require rewrites,
but those are mostly braindead and can likely be automated completely.
I usually just use vimsed.
(For languages nerds: zepto i a Lisp-1 and understands R5RS pretty well;
later dialects, not so much)
I would consider it a superset of Scheme at this point. Not necessarily because
of the language features, although a few of the types are not in the specs,
but because it actually has a good amount of libraries that are usable;
the lack of a "batteries included" Scheme (no to Chicken, no to Chibi) saddened
me for a long time.

So, what are zepto's killer features? It has a numeric tower that is even a bit
bigger than Schemes because I added small-ints, which are basically just the
biggest type that the target platform supports natively. They behave as you would
expect and should only ever be used if you dig wrap-arounds if they mean speed.

There is a module system. I will get to that later.

There are macros and continuations, the latter of which may be buggy. I don't really
use them, but they were fun and challenging to implement.

The last feature is that the interpreter exists natively and in the browser,
which means that after loading a Javascript file that is much too big and maybe
the prelude (you know it as clojure.core), unless you want to write everything
yourself, you can write zepto in the browser. It even has a basic Javascript
foreign function interface.

I am also building a compiler that is targetting both Erlang bytecode and machine
code at the moment, but that's still a long shot.

There are a lot of libraries already present, some of them essential, some just
nice to have; it is not entirely unusable.

Docs are virtually non-existant, or better, severely outdated. Don't expect much
in that department.

Now, to the module system. It is small, which means that it is only 65 lines of
code long. The lines are quite dense, though, I would say, denser than your average
Clojure code.

In a way it is easy. It is small, the interface is sleak and consistent.
It is hard to debug, though, which is annoying but can be remedied.

Defining a module looks like this. If you cannot read this, I can find you
a version with syntax highlighting and all of that fancy shmanzy stuff.

That's what using the module from before looks like.

Admittedly, the code from before was fairly verbose. Let's make it easier.

Let's go to the implementation. The last function, module-extend, is missing,
because I needed to fit the code on a slide and it's way too long as it is.

Okay, so now that you've seen it, let's pause for a moment and contemplate.
Is what I just showed you really cool and fancy? Not really.
Is it feature-rich? Hell no.
Is it better than Clojure's module system? In most cases, it isn't.
It is easier, though. It is consistent. It is easy to work on meaning, you can
easily read and write for and in it and there is almost no magic.

What should we use it for? A plugin system would be a fair use, I would argue.
It was entertaining to write, so if you're bored maybe give it a go yourself.
And, it is always nice to entertain and impress others with simple ideas.
Because it isn't hard, it really is not. It is a fairly simple implementation
of a fairly trivial thing. And that's what makes it powerful.

And I will leave it at that. If you have questions, comments or critical remarks,
tell me about it. Really.
