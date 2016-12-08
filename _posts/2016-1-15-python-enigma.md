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
The [Enigma Machine](wki.pe/Enigma_machine) was invented near the end of World War I by a German engineer named Arthur Scherbius. It is a [polyalphabetic cipher](wki.pe/Polyalphabetic_cipher) with a keyspace comparable to a \\(67\\) bit key (which is more than DES uses, and people used that until well into the \\(21^\text{st}\\) century). The machine works by routing electricity through rotating wheels. Keys mechanically rotate these wheels while closing electrical circuits that run through the wheels (twice) and then through a lampboard with letters `A` through `Z`. Every time you press a key on the keyboard, a lampboard lights up. (The present repository does not aim to emulate this behavior; only to encrypt and decrypt messages compatibly and efficiently.)

###### Mathematical Representation of the Commercial Enigma
Mathematically speaking, the Enigma Machine generates a permutation which is a *[involutory](wki.pe/Involution_(mathematics)) [derangement](wki.pe/Derangement)*. That is to say, for any given arrangement of the settings (key state), the Enigma Machine is a function from the [Roman alphabet](Roman_alphabet) (which we shall call `alphabet`) to itself and follows some very specific rules. If we call that function \\(\\sigma\\) then these rules can be stated as follows. For each \\(x\\) in `alphabet`:
1. \\(\\sigma(x)\\) is in `alphabet`.
2. \\(\\sigma(y)=x\\) for some \\(y\\) in `alphabet`.
3. \\(\\sigma(x)\\neq x\\).
4. \\(\\sigma(\\sigma(x)) = x\\).

The first two ensure that \\(\\sigma\\) is a **[permutation](wki.pe/Permutation)** and the third ensures it is a **derangment**. The fourth says that \\(\\sigma\\) is **involutory**. The involutary property was implemented as a convenience, but the implementation also gave rise to the derangement property, we shall see, was the thread that made the whole sweater unravel. The convenience we speak of was that the machine could encrypt and decrypt without making any changes. For example, if Alice sets her machine to `ABC` and types `HELLO WORLD` she might read something like `IOVDA YUEBH` from her lightboard. If Bob then sets his machine to `ABC` and keys in that ciphertext, `HELLO WORLD` will appear on his lightboard.

The derangment corresponding to each key setting is generated with something mathematicians call a *similarity*. Conventional current travels out of the positive terminal battery and through the circuit for the depressed key. It enters the first wheel and comes out at a different spot on the wheel (as a matter of fact each wheel deranges the letters, but this doesn't turn out to be important). The current continues in a similar fashion through wheels two and three (and perhaps four if we're using a German navy machine). This entire process can be regarded as one permutation \\(\\rho\\). For example, instead of saying 
\\[A \\stackrel{W\_1}{\\longmapsto} B \\stackrel{W\_2}{\\longmapsto} C \\stackrel{W\_3}{\\longmapsto} D \\]
we may just say that \\(A\\stackrel{\\rho}{\\longmapsto}D\\).

After the current maps through \\(\\rho\\) it hits something called the reflector, \\(\\tau\\). The reflector's electrical connections implement an involutory derangement by simply pairing off letters. For example perhaps \\(\\tau(A)=B\\) and \\(\\tau(B)=A\\). Finally the current travels *backwards* through \\(\\rho\\) and a fillament in the lightboard before completing its journey at the negative terminal of the battery. They key thing to understand is that this whole process guarantees that \\(\\sigma\\) is *also* an involutory derangement. To see why, imagine the journey of a key mapped through the entire process (\\(\\sigma\\)), twice. Here is a plausible example: we enter `A` on the keyboard. First \\(\\rho(A) = B\\) takes us through the rotors, then \\(\\tau(B)=C\\) takes us through the reflector. When traveling *backwards* through the rotors, we are mapping through \\(\\rho^{-1}\\). This is a slightly fancy way of saying that if \\(\\rho(A)=B\\) then \\(\\rho^{-1}(B)=A\\). This behavior is exactly what you would expect from connected wires. So finally \\(\\rho^{-1}(C)=D\\) and we have encrypted \\(\\sigma(A)=D\\) (so `D` lights up on our machine). In mathematical notation, we would say \\(\\sigma=\\rho^{-1}\\tau\\rho\\). This type of relationship is what mathematicians like to call a **similarity**. It is a good exercise to convince oneself that \\(\\rho\\) and \\(\\sigma\\) are actually permutations (i.e. satisfy properties one and two).

Now let's see what happens when we press `D` on the keyboard (with the exact same positions for all the rotors). First \\(\\rho(D) = C\\) (because it is the inverse of \\(\\rho^{-1}\\)). Then \\(\\tau(C )=B\\) (since \\(\\tau\\) is involutory). Finally \\(\\rho^{-1}(B)=A\\) (again, by definition of \\(\\rho^{-1}\\)). Viola! This shows that \\(\\sigma\\) is *also* involutory. All that remains is showing that \\(\\sigma\\) is a derangement. To see this we suppose that \\(\\sigma(A)=A\\) and try to figure out how such a thing might happen.

\\[ A \\stackrel{\\rho}{\\longmapsto} B \\stackrel{\\tau}{\\longmapsto} C \\stackrel{\\rho^{-1}}{\\longmapsto} A\\]

But we can already see that this is impossible because \\(\\rho(A)=B\\) and \\(\\rho^{-1}(C )=A\\). This contradicts the definition of \\(\\rho^{-1}\\)! We might attempt to resolve this by supposing that \\(B=C\\), but then \\(\\tau\\) would not be a derangement! (Incidentally, the enigma machine would have been far more secure while still maintaining the convenience of property four if the inventor had simply accepted this fact and allowed the reflector to map two letters to themselves. Inventing cryptography is hard.)

###### The Military Enigma

We have seen that in the commercial enigma a user presses a key sending current through a physical manifestation of an involutory derangement and then through a light bulb. More sophisticated versions of the machine (including those commonly used by the German military) included some extra settings that increase the keyspace. This was necessary as the commercial Engima machine only had \\(3!\\cdot 26^3 = 105456\\) different key settings. The rotors could be removed and replaced in any order (hence the \\(3!\\)), and each rotor could be set to any of \\(26\\) different positions (one for each letter in `alphabet`).  It would take a long time to check all of those settings without a computer, but it was definitely feasible for a determined eavesdropper (like, say, the Allied Forces).

One way of increasing the keyspace was to introduce extra rotors. The German army, for instance chose from a set of \\(5\\) rotors instead of \\(3\\). The navy chose \\(4\\) out of a total of \\(8\\). Still, this only increased the number of possible keys to \\(\\frac{8!}{4!}26^4\\). That's about \\(768\\) million: \\(5\\) orders of magnitude larger than the commercial machine, but only equivalent to about a \\(30\\) bit key.

The real boost to the keyspace came from the addition of a plugboard. Operators could open a door on the front of the machine to reveal \\(26\\) sockets for up to \\(13\\) wires (although the standard practice was to use exactly \\(10\\)). Current travelled through these wires after the key press on the way to the rotor and once again after the reverse path through the rotors on the way to the lightboard. The effect was an *additional* permutation in the system. But this one can be set up in a staggeringly large number of ways (much more than \\(26\\)). The number of ways to configure a plugboard with \\(10\\) wires in \\(26\\) sockets is:

\\[ \frac{1}{10!}\\cdot\\binom{26}{2}\\cdot\\binom{24}{2}\\cdot\\binom{22}{2}\\dots\\binom{10}{2}\\cdot\\binom{8}{2} \\]

That's 151 trillion (a trillion is a million million) different keys. Multiply that by the keyspace from the standard machine with \\(5\\) rotors and \\(3\\) slots, and you've got . The plugboard (and extra rotor) do not change much about the substance arguments made above about the special properties of the final permutation, \\(\\sigma\\). It is still an involutory derangement because \\(\\sigma\\) is still *similar* to \\(\\tau\\). All told the military version of the enigma machine was approximately equivalent to a \\(67\\) bit key (\\(76\\) for the naval M4 with its extra rotors). Even checking one million keys per second (which is quite a bit faster than I can do on my home computer), it would take over \\(4.5\\) million years to check all the possibilities!

### Breaking the Enigma Machine

### Future Work
The repository will eventually contain an easy-to-use implementation of the Brittish Bombe machine. It will accept a ciphertext message along with a *crib* and a location for that crib. After testing all of the wheel settings for *contradictions*, the machine will return a set of plausible wheel settings.

###### Wish List
* *n-gram* testing to screen plausible keys.
* *hill-climbing algorithm*
* Frequent German words dictionary for searching without a key.
* Database of historical Enigma messages.
