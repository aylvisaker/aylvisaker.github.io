---
layout: post
title: "Python Enigma"
categories: [projects]
tags: [puzzles]
description: ""
---

Python Enigma
============
[The repository](github.io/aylvisaker/python-enigma) stores a python implementation of the Enigma Machine. It creates a class called `enigma` which is fully compatible with historical machines with a variety of different rotor configurations.

  * [About The Enigma Machine](#about-the-enigma-machine)
    * [Operation](#operation)
    * [Mathematical Representation](#mathematical-representation)
    * [Military Enigma and Keyspace Analysis](#military-enigma-and-keyspace-analysis)
  * [Breaking Enigma](#breaking-enigma)
  * [Code](#code)
  * [Future Work](#future-work)

## About the Enigma Machine
#### Operation
The [Enigma Machine](http://wki.pe/Enigma_machine) was invented near the end of World War I by a German engineer named Arthur Scherbius. It is a mechanical encryption device with a key space comparable to a $67$ bit key (which is significantly more than an algorithm called DES uses, and people were using that at the turn of the century). The machine works by routing electricity through rotating wheels, called rotors. A key press mechanically rotates these wheels while closing electrical circuits that drive current through the wires inside them and then through a lampboard with letters `A` through `Z`. Every time an operator presses a key on the keyboard, a lampboard lights up.

It is helpful to have a specific picture of how this circuit works in order to understand how the cipher was broken. For now, we'll ignore the step function which rotates the wheels. [Conventional current](https://en.wikipedia.org/wiki/Electric_current#Current) travels out of the positive terminal battery through a circuit completed by the depressed key (with each key completing a different circuit). Current then enters the first rotor on the right side at a location around its perimeter determined by the letter pressed on the keyboard and comes out at a different location on its left perimeter. The rotor contains $26$ wires connecting the $26$ locations on its right side to the ones on its left. Those wires determine where the current goes. This new location represents a new letter and the first step in the encryption process. The current continues in a similar fashion through rotors two and three (and perhaps four if we're using a German navy machine).

After its journey through the rotors, current hits something called a reflector. The reflector functions in much the same way that the rotors do. It has $13$ wires inside of it connecting $26$ different spots on its right side (which are connected to the final rotor). This effectively turns the current around and sends it on a journey backwards through the rotors. The location where the current finally re-exits the first rotor determines which lamp lights up on the lightboard.

Now this would all be quite over-elaborate if the rotors didn't also move (any given setting would be an instance of a [substitution ciphers](http://wki.pe/Substitution_cipher) which are trivially broken by [frequency analysis](http://wki.pe/Frequency_analysis)). When the wheels rotate, they effectively scramble the entire circuit. So if Alice sets her machine to `ABC`  (indicating the starting configuration of the rotors) and types `HELLO WORLD` she might read something like `IOVDA YUEBH`. Notice that the letter `L` got mapped to a different letter each time it appeared in the message! Cryptographers call that a [polyalphabetic cipher](http://wki.pe/Polyalphabetic_cipher). 

Each wheel only has $26$ positions, but each key press mechanically drives the first (rightmost) wheel forward by $\frac{1}{26}$ of a full rotation. At a certain point in this process, the position of the right wheel triggers a turn in its left neighbor. Similarly, at some point that rotor will trigger a turn in the third rotor. The reflector does not turn, but you would have to be encrypting something almost $26^3 = 17576$ characters long before the machine returned to its original position (the aforementioned fourth rotor in the naval M4 did not turn like the other three). The reason that we say *almost* is a mechanical artifact that causes the wheels to *double step* occasionally. This needs to be accounted for in software emulating Enigma, but is not relevant to the present analysis.

#### Mathematical Representation
For now we restrict our attention to the version of the machine that was made available commercially. Mathematically speaking, the the Enigma generates a permutation which is an *involutory derangement*. That is to say, for any given state of the machine's settings, the Enigma Machine is a function (called a *permutation*) from the [Roman alphabet](Roman_alphabet) (which we shall call `alphabet`) to itself and follows some very specific rules. If we call that function $\sigma$ then these rules can be stated as follows. For each $x$ in `alphabet`:

1. $\sigma(x)$ is in `alphabet`.
2. $\sigma(y)=x$ for some $y$ in `alphabet`.
3. $\sigma(x)\neq x$.
4. $\sigma(\sigma(x)) = x$.

The first two ensure that $\sigma$ is a [permutation](http://wki.pe/Permutation), the third ensures $\sigma$ is a [derangement](http://wki.pe/Derangement), and the fourth ensures $\sigma$ is an [involution](http://wki.pe/Involution_(mathematics)). The involutary property was implemented as a convenience, but the implementation also gave rise to the derangement property, we shall see, was the thread that made the whole sweater unravel. The convenience we speak of was that the machine could encrypt and decrypt without making any changes. For example, if Alice sets her machine to `ABC` and types `HELLO WORLD` she might read something like `IOVDA YUEBH` from her lightboard. If Bob then sets his machine to `ABC` and keys in that ciphertext, `HELLO WORLD` will appear on his lightboard.

The derangement corresponding to each key setting is generated with something mathematicians call a *similarity*. The path of current through the machine can be regarded as one permutation $\rho$. For example, instead of saying 
$$ A \stackrel{R_1}{\longmapsto} B \stackrel{R_2}{\longmapsto} C \stackrel{R_3}{\longmapsto} D $$
we may just say that $A\stackrel{\rho}{\longmapsto}D$.

After the current maps through $\rho$ it hits reflector, which we shall call $\tau$. The reflector's electrical connections implement an involutory derangement by simply pairing off letters. For example perhaps $\tau(A)=B$ and $\tau(B)=A$. Finally the current travels *backwards* through $\rho$ and a filament in the lightboard before completing its journey at the negative terminal of the battery. They key thing to understand is that this process guarantees that encryption $\sigma$ is *also* an involutory derangement. To see why, imagine the journey of a key mapped through $\sigma$, twice (once for encryption, once for decryption). Here is a plausible example: we enter `A` on the keyboard. First $\rho(A) = B$ takes us through the rotors, then $\tau(B)=C$ takes us through the reflector. When traveling *backwards* through the rotors, we are mapping through $\rho^{-1}$. This is a slightly fancy way of saying that if $\rho(A)=B$ then $\rho^{-1}(B)=A$. This behavior is exactly what you would expect from connected wires. So finally $\rho^{-1}(C)=D$ and we have encrypted $\sigma(A)=D$ (so `D` lights up on our machine). In mathematical notation, we would say $\sigma=\rho^{-1}\tau\rho$. When this is true, we say that $\sigma$ and $\rho$ are [similar](http://wki.pe/Matrix_similarity) permutations. It is a good exercise to convince oneself that $\rho$, $\rho^{-1}$, and $\sigma$ are actually permutations (i.e. satisfy properties one and two).

Now let's see what happens when we press `D` on the keyboard (with the exact same positions for all the rotors). First $\rho(D) = C$ (because $\rho$ is the inverse of $\rho^{-1}$, another good exercise for the reader). Then $\tau(C )=B$ since $\tau$ is an involution. Finally $\rho^{-1}(B)=A$ by definition of $\rho^{-1}$. Viola! This shows that $\sigma$ is also an involution. In fact this argument shows that any permutation similar to an involution must also be involutary. All that remains is to show that $\sigma$ is a derangement. To see this we suppose that $\sigma(A)=A$ and try to figure out how such a thing might happen. It would have to look something like this:
$$ A \stackrel{\rho}{\longmapsto} B \stackrel{\tau}{\longmapsto} C \stackrel{\rho^{-1}}{\longmapsto} A$$
But we can already see that this is impossible because $\rho(A)=B$ and $\rho^{-1}(C )=A$, which contradicts the definition of $\rho^{-1}$. We might resolve this by supposing that $\tau(B)=B$ instead of $C$, but then $\tau$ would not be a derangement. Indeed, any permutation similar to a derangement is itself a derangement.

### Military Enigma and Keyspace Analysis

We have seen that in the commercial enigma a user presses a key sending current through a physical manifestation of an involutory derangement and then through a light bulb. More sophisticated versions of the machine (including those commonly used by the German military) included some extra settings that increase the key space. This was necessary as the commercial Enigma machine only had $3!\cdot 26^3 = 105456$ different key settings. The rotors could be removed and replaced in any order (hence the $3!$), and each rotor could be set to any of $26$ different positions (one for each letter in `alphabet`).  It would take a long time to check all of those settings without a computer, but it was definitely feasible for a determined eavesdropper (like, say, the Allied Forces).

One way of increasing the key space was to introduce extra rotors. The German army, for instance, chose from a set of $5$ rotors instead of $3$, which increased the number of configurations by a factor of $10$ to about $1$ million. The navy chose $3$ out of $8$ and selected a fourth thin rotor from a set of $2$, bringing the size of the key space up to $2\cdot\frac{8!}{5!}\cdot26^4$. That's a monstrous $307$ million different possible key settings. The navy also had a second reflector, but we don't count this in our computations because in practice it was never changed. Each rotors also has a setting that essentially controls the point in the rotation at which the rotor triggers a step for its left neighbor. We don't count this in the key space either. Even with randomly selected *Ringstellung* the first few characters will decrypt correctly (until the rightmost ring incorrectly triggers a rotation of the next rotor, or fails to do so). After seeing this it isn't difficult for a cryptographer to narrow down the choices and decrypt the rest of the message.

The real boost to the key space came from the addition of a plugboard. Enigma operators could open a door on the front of the machine to reveal $26$ sockets for up to $13$ wires (although the standard practice was to use exactly $10$). Current travelled through these wires after the keyboard on the way to the rotor and once again after the reverse path through the rotors on the way to the lightboard. This adds an additional permutation (and its inverse) to the system. We note here that the plugboard (and extra rotor) do not change much about the substance of the arguments made above about the special properties of $\sigma$. The permutation $\sigma$ is still an involutory derangement because it is still *similar* to $\tau$. The number of ways to configure a plugboard with $10$ wires in $26$ sockets is staggeringly large:

$$ \frac{1}{10!}\cdot\binom{26}{2}\cdot\binom{24}{2}\cdot\binom{22}{2}\dots\binom{10}{2}\cdot\binom{8}{2}  $$

That's over $150$ trillion different plugboard variations. Multiply that by the number of ways to select and configure the rotors and you have a very very large key space.  All told the army version of the Enigma had about $159$ quintillion different key settings (that's a $159$ million million million). The navel M4 is almost $300$ times larger with over $46289$ quintillion keys: $ 46\,289\,896\,079\,431\,036\,032\,000 $. Even checking one million keys per second (which is quite a bit faster than I can do on my home computer), it would take over $5$ million years to check all the possible key settings for an army Enigma. If you had that many sheets of paper to stack, the stack would reach Proxima Centuary. And back. Fifty-eight times.

## Breaking the Enigma Machine

## Code
```python
print('hello world')
for x in range(y):
	print(3 + 4*5)
```

## Future Work
The repository will eventually contain an easy-to-use implementation of the British Bombe machine. It will accept a ciphertext message along with a crib and a location for that crib. After testing all of the wheel settings for contradictions, the machine will return a set of plausible wheel settings.

##### Wish List
* automatic crib-sliding
* *n-gram* testing to screen plausible keys.
* *hill-climbing algorithm*
* Frequent German words dictionary for searching without a key.
* Database of historical Enigma messages.
