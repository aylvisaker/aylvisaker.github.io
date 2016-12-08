---
layout: post
title: "Python Enigma"
categories: [projects]
tags: [puzzles]
description: ""
---

# python-enigma
[The repository](github.io/aylvisaker/python-enigma) stores a python implementation of the Enigma Machine. It creates a class called `enigma` which is fully compatible with historical machines with a variety of different rotor configurations.

### About the Enigma Machine
###### Physical Operation
The [Enigma Machine](wki.pe/Enigma_machine) was invented near the end of World War I by a German engineer named Arthur Scherbius. It is a polyalphabetic cipher with a keyspace comparable to a 77 bit key. The machine works by routing electricity through rotating wheels. Keys mechanically rotate these wheels while closing electrical circuits that run through the wheels (twice) and then through a lampboard with letters A through Z. Every time you press a key on the keyboard, a lampboard lights up. (The present repository does not aim to emulate this behavior; only to encrypt and decrypt messages compatibly and efficiently.)

###### Mathematical Representation of the Commercial Enigma
Mathematically speaking, the Enigma Machine generates a permutation which is a *derangement of order 2*. That is to say, for any given arrangement of the settings (key state), the Enigma Machine is a function from the [Roman alphabet](Roman_alphabet) (which we shall call `alphabet`) to itself and follows some very specific rules. If we call that function \\(\\sigma\\) then these rules can be stated as follows. For each \\(x\\) in `alphabet`:
1. \\(\\sigma(x)\\) is in `alphabet`.
2. \\(\\sigma(y)=x\\) for some \\(y\\) in `alphabet`.
3. \\(\\sigma(x)\\neq x\\).
4. \\(\\sigma(\\sigma(x)) = x\\).

The first two ensure that \\(\\sigma\\) is a permutation and the third ensures it is a *derangment*. The fourth says that \\(\\sigma\\) has *order 2*. Property four was implemented as a convenience, but the implementation also gave rise to property three which, we shall see, was the thread that made the whole sweater unravel. The convenience we speak of was that the machine could encrypt and decrypt without making any changes. For example, if Alice sets her machine to `ABC` and types `HELLO WORLD` she might read something like `IOVDA YUEBH` from her lightboard. If Bob then sets his machine to `ABC` and keys in that ciphertext, `HELLO WORLD` will appear on his lightboard.

The derangments for each key setting are generated with something called a *similarity*. Conventional current travels out of the positive terminal battery and through the circuit for the depressed key. It enters the first wheel and comes out at a different spot on the wheel (as a matter of fact each wheel deranges the letters, but this doesn't turn out to be important). The current continues in a similar fashion through wheels two and three (and perhaps four if we're using a German navy machine). This entire process can be regarded as one permutation \\(\\rho\\). For example, instead of saying 
\\[A \\stackrel{W\_1}{\\longmapsto} B \\stackrel{W\_2}{\\longmapsto} C \\stackrel{W\_3}{\\longmapsto} D\\]
we may just say that \\(A\\stackrel{\\rho}{\\longmapsto}D\\).

After the current maps through \\(\\rho\\) it hits something called the reflector, \\(\\tau\\). The reflector's circuitry implements a derangement of order 2 (which, we shall see, is **very** important). Finally the current travels *backwards* through \\(\\rho\\) and a fillament in the lightboard before completing its journey at the negative terminal of the battery. They key thing to understand is that this whole process guarantees that \\(\\sigma\\) is *also* a derangement of order 2. To see why, imagine the journey of a key mapped through the entire process (\\(\\sigma\\)), twice. Here is a plausible example: we enter `A` on the keyboard. First \\(\\rho(A) = B\\) takes us through the rotors, then \\(\\tau(B)=C\\) takes us through the reflector. When traveling *backwards* through the rotors, we are mapping through \\(\\rho^{-1}\\). This is a slightly fancy way of saying that if \\(\\rho(A)=B\\) then \\(\\rho^{-1}(B)=A\\). This is behavior is exactly what you would expect from connected wires. So finally \\(\\rho^{-1}(C)=D\\) and we have encrypted \\(\\sigma(A)=D\\) (so `D` lights up on our machine). In mathematical notation, we would say \\(\\sigma=\\rho^{-1}\\tau\\rho\\). This type of relationship is what mathematicians like to call a **similarity**. It is a good exercise to convince oneself that \\(\\rho\\) and \\(\\sigma\\) are actually permutations (i.e. satisfy properties one and two).

Now let's see what happens when we press `D` on the keyboard (with the exact same positions for all the rotors). First \\(\\rho(D) = C\\) (because it is the inverse of \\(\\rho^{-1}\\)). Then \\(\\tau(C )=B\\) (since \\(\\tau\\) has order 2). Finally \\(\\rho^{-1}(B)=A\\) (by definition of \\(\\rho^{-1}\\). Viola! This shows that \\(\\sigma\\) *also* has order 2. Showing that \\(\\sigma\\) is also a derangement is all that remains. To see this we suppose that \\(\\sigma(A)=A\\) and try to figure out how this might happen.

\\[ A \\stackrel{\\rho}{\\longmapsto} B \\stackrel{\\tau}{\\longmapsto} C \\stackrel{\\rho^{-1}}{\\longmapsto} A\\]

But we can already see that this is impossible because \\(\\rho(A)=B\\) and \\(\\rho^{-1}(C )=A\\). This contradicts the definition of \\(\\rho^{-1}\\)! We might attempt to resolve this by supposing that \\(B=C\\), but then \\(\\tau\\) would not be a derangement! (Incidentally, the enigma machine would have been far more secure while still maintaining the convenience of property four if the inventor had simply accepted this fact and allowed the reflector to map two letters to themselves. Inventing cryptography is hard.)

### Future Work
The repository will eventually contain an easy-to-use implementation of the Brittish Bombe machine. It will accept a ciphertext message along with a *crib* and a location for that crib. After testing all of the wheel settings for *contradictions*, the machine will return a set of plausible wheel settings.

###### Wish List
* *n-gram* testing to screen plausible keys.
* *hill-climbing algorithm*
* Frequent German words dictionary for searching without a key.
* Database of historical Enigma messages.
