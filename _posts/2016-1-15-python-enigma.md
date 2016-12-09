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
The [Enigma Machine](wki.pe/Enigma_machine) was invented near the end of World War I by a German engineer named Arthur Scherbius. It is a [polyalphabetic cipher](wki.pe/Polyalphabetic_cipher) with a key space comparable to a $67$ bit key (which is more than DES uses, and people used that until well into the $21^\text{st}$ century). The machine works by routing electricity through rotating wheels, called rotors. A key press mechanically rotates these rotors while closing electrical circuits that run through the wheels (twice) and then through a lampboard with letters `A` through `Z`. Every time an operator presses a key on the keyboard, a lampboard lights up.

###### Mathematical Representation of the Commercial Enigma
Mathematically speaking, the Enigma Machine generates a permutation which is an *[involutory](wki.pe/Involution_(mathematics)) [derangement](wki.pe/Derangement)*. That is to say, for any given state of the machine's settings, the Enigma Machine is a function from the [Roman alphabet](Roman_alphabet) (which we shall call `alphabet`) to itself and follows some very specific rules. If we call that function $\sigma$ then these rules can be stated as follows. For each $x$ in `alphabet`:
1. $\sigma(x)$ is in `alphabet`.
2. $\sigma(y)=x$ for some $y$ in `alphabet`.
3. $\sigma(x)\neq x$.
4. $\sigma(\sigma(x)) = x$.

The first two ensure that $\sigma$ is a **[permutation](wki.pe/Permutation)** and the third ensures it is a **derangment**. The fourth says that $\sigma$ is **involutory**. The involutary property was implemented as a convenience, but the implementation also gave rise to the derangement property, we shall see, was the thread that made the whole sweater unravel. The convenience we speak of was that the machine could encrypt and decrypt without making any changes. For example, if Alice sets her machine to `ABC` and types `HELLO WORLD` she might read something like `IOVDA YUEBH` from her lightboard. If Bob then sets his machine to `ABC` and keys in that ciphertext, `HELLO WORLD` will appear on his lightboard.

The derangment corresponding to each key setting is generated with something mathematicians call a *similarity*. Conventional current travels out of the positive terminal battery and through the circuit for the depressed key. It enters the first wheel and comes out at a different spot on the wheel (as a matter of fact each wheel deranges the letters, but this doesn't turn out to be important). The current continues in a similar fashion through wheels two and three (and perhaps four if we're using a German navy machine). This entire process can be regarded as one permutation $\rho$. For example, instead of saying 
$$A \stackrel{W\_1}{\longmapsto} B \stackrel{W\_2}{\longmapsto} C \stackrel{W\_3}{\longmapsto} D $$
we may just say that $A\stackrel{\rho}{\longmapsto}D$.

After the current maps through $\rho$ it hits something called the reflector, $\tau$. The reflector's electrical connections implement an involutory derangement by simply pairing off letters. For example perhaps $\tau(A)=B$ and $\tau(B)=A$. Finally the current travels *backwards* through $\rho$ and a fillament in the lightboard before completing its journey at the negative terminal of the battery. They key thing to understand is that this whole process guarantees that $\sigma$ is *also* an involutory derangement. To see why, imagine the journey of a key mapped through $\sigma$, twice (once for encryption, once for decryption). Here is a plausible example: we enter `A` on the keyboard. First $\rho(A) = B$ takes us through the rotors, then $\tau(B)=C$ takes us through the reflector. When traveling *backwards* through the rotors, we are mapping through $\rho^{-1}$. This is a slightly fancy way of saying that if $\rho(A)=B$ then $\rho^{-1}(B)=A$. This behavior is exactly what you would expect from connected wires. So finally $\rho^{-1}(C)=D$ and we have encrypted $\sigma(A)=D$ (so `D` lights up on our machine). In mathematical notation, we would say $\sigma=\rho^{-1}\tau\rho$. This type of relationship is what mathematicians like to call a **similarity**. It is a good exercise to convince oneself that $\rho$ and $\sigma$ are actually permutations (i.e. satisfy properties one and two).

Now let's see what happens when we press `D` on the keyboard (with the exact same positions for all the rotors). First $\rho(D) = C$ (because it is the inverse of $\rho^{-1}$). Then $\tau(C )=B$ (since $\tau$ is involutory). Finally $\rho^{-1}(B)=A$ (by definition of $\rho^{-1}$). Viola! This shows that $\sigma$ is involutory as well. In fact this argument shows that any permutation must be involutary if it is similar to an involutary permutation. All that remains is to show that $\sigma$ is a derangement. To see this we suppose that $\sigma(A)=A$ and try to figure out how such a thing might happen.
$ A \stackrel{\rho}{\longmapsto} B \stackrel{\tau}{\longmapsto} C \stackrel{\rho^{-1}}{\longmapsto} A$
But we can already see that this is impossible because $\rho(A)=B$ and $\rho^{-1}(C )=A$. This contradicts the definition of $\rho^{-1}$! We might attempt to resolve this by supposing that $B=C$, but then $\tau$ would not be a derangement! (Incidentally, the enigma machine would have been far more secure while still maintaining the convenience of the involutary property if the inventor had simply accepted this fact and allowed the reflector to map two letters to themselves. Inventing cryptography is hard.)

###### The Military Enigma

We have seen that in the commercial enigma a user presses a key sending current through a physical manifestation of an involutory derangement and then through a light bulb. More sophisticated versions of the machine (including those commonly used by the German military) included some extra settings that increase the key space. This was necessary as the commercial Engima machine only had $3!\cdot 26^3 = 105456$ different key settings. The rotors could be removed and replaced in any order (hence the $3!$), and each rotor could be set to any of $26$ different positions (one for each letter in `alphabet`).  It would take a long time to check all of those settings without a computer, but it was definitely feasible for a determined eavesdropper (like, say, the Allied Forces).

One way of increasing the key space was to introduce extra rotors. The German army, for instance, chose from a set of $5$ rotors instead of $3$, which increased the number of configurations by a factor of $10$ to about $1$ million. The navy chose $3$ out of $8$ and selected a fourth thin rotor from a set of $2$, bringing the size of the key space up to $2\cdot\frac{8!}{5!}\cdot26^4$. That's a monsterous $307$ million different possible key settings. The navy also had a second reflector, but we don't count this in our computations because in practice it was never changed. Each rotors also has a setting that essentially controls the point in the rotation at which the rotor triggers a step for its left neighbor (like you would see the odometer in a car doing). We don't count this in the key space either though. Even with randomly selected *ringstellung* the first few characters will decrypt (until the rightmost ring mistakenly triggers a rotation of the next rotor or fails to do so when it is supposed to). After seeing this it isn't difficult for a cryptographer to narrow down the choices and decrypt the rest of the message.

The real boost to the key space came from the addition of a plugboard. Enigma operators could open a door on the front of the machine to reveal $26$ sockets for up to $13$ wires (although the standard practice was to use exactly $10$). Current travelled through these wires after the keyboard on the way to the rotor and once again after the reverse path through the rotors on the way to the lightboard. This adds an additional permutation (and its inverse) to the system. We note here that the plugboard (and extra rotor) do not change much about the substance of the arguments made above about the special properties of $\sigma$. The permutation $\sigma$ is still an involutory derangement because it is still *similar* to $\tau$. The number of ways to configure a plugboard with $10$ wires in $26$ sockets is staggeringly large:

$$ \frac{1}{10!}\cdot\binom{26}{2}\cdot\binom{24}{2}\cdot\binom{22}{2}\dots\binom{10}{2}\cdot\binom{8}{2} $$

That's over $150$ trillion different plugboard variations. Multiply that by the number of ways to select and configure the rotors and you have a very very large key space.  All told the army version of the Enigma had about $159$ quintillion different key settings (that's a $159$ million million million). The navel M4 is almost $300$ times larger with over $46289$ quintillion keys. Even checking one million keys per second (which is quite a bit faster than I can do on my home computer), it would take over $5$ million years to check all the possibile key settings for an army Enigma.

### Breaking the Enigma Machine

### Future Work
The repository will eventually contain an easy-to-use implementation of the Brittish Bombe machine. It will accept a ciphertext message along with a *crib* and a location for that crib. After testing all of the wheel settings for *contradictions*, the machine will return a set of plausible wheel settings.

###### Wish List
* *n-gram* testing to screen plausible keys.
* *hill-climbing algorithm*
* Frequent German words dictionary for searching without a key.
* Database of historical Enigma messages.
