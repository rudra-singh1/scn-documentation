--
layout: default
title: Wireless Communication
parent: Learn
nav_order: 1
---

# Crash Course in Wireless Communication

Authored by: Dominick Ta
Last updated: June 18th, 2021

TODO:
- [ ] Expand on transmit/receiving in antennas section
- [ ] Finish communication section
- [ ] Finish protocols section

This is a crash course in wireless communication: the magical way that we humans send information (e.g. music, messages, text, videos) to each other over long distances, without a wire. 

Wireless communication is primarily powered by radio waves, a physical thing that we as humans cannot see or touch. In this article you will gain a layman's understanding of how wireless communication works. Specifically, this article will cover the following broad topics:

- Radio waves and the physics behind it
- Antennas
- How data is communicated via radio waves
- Examples of wireless communication technologies

This is not meant to be a comprehensive article, so there will be a lot of simplifications, analogies, and informal explanations. Please let me know at domta@cs.uw.edu if you see any inaccuracies, misconceptions, or misnomers.

---

## Radio waves

Radio waves are just oscillations of energy that exist throughout our environment. They can be generated naturally by lightning, or artifically by equipment made by humans such as cell phones.

The technical definition of radio waves are that they are a form of 'electromagnetic radiation'. Other types of electromagnetic radiation include visible light (what we get from the sun), infrared, and X-rays.

![Image of a radio surrounded by pulsating radio waves](https://cdn.britannica.com/s:800x450,c:crop/86/214986-138-D4D194FD/How-radio-works-overview-radio-waves-frequency-amplitude-modulation.jpg)

### How radio waves are created

To understand why radio waves are considered a type of electromagnetic radiation, it is helpful to know how we generate radio waves. ***This section is not important to fully understand, but it is good to skim to have a basic idea of how this stuff works.***

We create radio waves by **moving electrons back and forth** within an electrically conductive object (an object made of material that allows electrons to move freely, such as metals).

This works because electrons have special properties:
- All electrons have '[electric fields](https://en.wikipedia.org/wiki/Electric_field)' surrounding them that attracts and repels other charged particles.
- All moving electrons produce '[magnetic fields](https://en.wikipedia.org/wiki/Magnetic_field)' that are the basis for how magnets work.
- Therefore, when electrons move there are two fields (electric & magnetic) present that combine to create creates an '[electromagnetic field](https://en.wikipedia.org/wiki/Electromagnetic_field)'.

By moving an electron back and forth in an oscillating (repetitive) motion, these electromagnetic fields are constantly being disturbed/moved in a way that creates **electromagnetic waves** or equivalently, [electromagnetic radiation](https://en.wikipedia.org/wiki/Electromagnetic_radiation).

In this GIF below, we see the charge of an antenna changing, representing electrons moving back and forth within the antenna. This creates pulsating electromagnetic waves. **Note: in this particular GIF, it is actually only showing the pulsating electric fields.**

![Animated image of waves pulsating from an antenna](https://upload.wikimedia.org/wikipedia/commons/a/a6/Dipole_xmting_antenna_animation_4_408x318x150ms.gif)

In this GIF below, we are able to fully see an electromagnetic wave with a 3D representation. The red component of the wave represents the electric field, while the blue component represents the magnetic field. Notice how they are perpendicular to each other! *This is also why the GIF above didn't show both fields: it was a 2D representation!*
![3D gif of electromagnetic waves, showing both the electric fields and magnetic fields](https://upload.wikimedia.org/wikipedia/commons/4/4c/Electromagneticwave3D.gif)

In this section, we learned about radio waves. In essence, you can just think of radio waves as physical phenomenon that look like these sine waves:
![GIF of a basic sinusoidal wave at a constant frequency](https://i.gifer.com/7myX.gif)

With the proper hardware and knowledge, we can create any type of wave we want! We can vary frequencies (how fast the waves go by), amplitudes (how tall/powerful the waves are), and phases (what position within the loop the wave is in).

### Frequency & Wavelength
It turns out that a fundamental characteristic of any given radio wave is its frequency. Therefore, it's important to know what 'frequency' means, and how it relates to the concept of 'wavelength'.

[Frequency](https://en.wikipedia.org/wiki/Frequency) is a measurement of how often something happens. In the case of radio waves, it measures how many cycles of a radio wave occurs in a certain amonut of time. Frequency is measured in the units of **hertz (Hz)**, which represents cycles per second. If we had a radio wave that passed through 10 complete cycles in a minute, it would have a frequency of 0.16Hz (10 cycles per 60 seconds).

[Wavelength](https://en.wikipedia.org/wiki/Wavelength) measures the physical length of a single cycle in a radio wave. If we could see radio waves and measure it, and we saw that there was a distance of 10-feet between two of the peaks in a radio wave, then that radio wave would have a wavelength of 10 feet.

![Image showcasing a wave with high frequency & short wavelength, and a wave with low frequency & long wavelength](https://cdn.britannica.com/83/194283-004-37696A2F.jpg)

In the image above, we can see a natural relationship between frequency and wavelength: a faster frequency means that you will have shorter wavelengths! Or conversely, longer wavelengths means that there is a slower frequency! When one value goes up, the other value has to go down; this is called an **inverse relationship**.

So if I told you that I had a radio wave with a very high frequency, you could figure out that my radio wave has very small wavelengths. And actually, if I told you exactly what frequency my radio waves travelled at, you'd be able to figure out the exact size of the wavelength!

For example, if I told you I had a radio wave with a frequency of 10Hz, you would be able to figure out that my radio wave has a wavelength of approximately 30,000,000 meters.

This is because radio waves are a physical thing, so they always travel through air at the same speed: the speed of light. This means we can consistently convert back and forth between frequency and wavelength with some basic algebra.

The basic equation, where c is the speed of light (3.0 x 10^8 m/s), is:
frequency (Hz) * wavelength (m) = c

So given a radio wave with a frequency of 10Hz, to solve for wavelength, I just need to take the speed of light and divide by 10! 30,000,000 divided by 10 gives a value of a wavelength of 30,000,000 meters.

To summarize this section, frequency and wavelength are important properties of radio waves. Although they describe different aspects of a radio wave, they are essentially synonymous because we can easily convert between the two.

When people are talking about radio waves, you may hear them talk in terms of frequencies (e.g. megahertz, gigahertz) or you may hear them talk in terms of wavelengths (e.g. meters).

In the next section, we'll see why exactly frequency is such an important characteristic of radio waves.

### How we distinguish between radio waves
An important thing to remember about radio waves is that nowadays they **surround us everywhere** we go! Radio waves are powerful because they can **go through obstacles** like walls, and they can potentially propagate over **huge distances** at a relatively cheap cost. 

Radio waves are used to communicate information in technologies such as GPS, WiFi, Bluetooth, cell phones, music radio stations, and more.

The problem with this is that these radio waves interfere with each other and combine together to become a jumbled mess! They are all co-existing within the same space, and they can't avoid each other. 

In real life we don't get to have nice, simple, isolated radio wave like in the previous sections, we get a mix of radio waves coming all at once! And somehow, we have to find and narrow down the signal we care about.

![GIF showcasing a bunch of random radio waves](https://miro.medium.com/max/1614/1*OyERBXcdPThv79uGu0QYGw.gif)

Imagine you are at an airport and you're trying to talk to your friend, but it is super crowded and theres people talking and shouting all around you. It would be super hard to hear your friend and have a conversation! 

*(In this situation the people represent devices that use radio waves, the sound waves represent radio waves, and the voices & words represent the data we want to transmit)*

But if your friend talks loud enough, even though your ear is full of noise from everyone else in the airport, you can still figure out what your friend is trying to tell you. This is because you're smart enough to recognize what your friend's voice sounds like and you can focus on that voice. We can do the same thing with radio waves of different frequencies!

*(Being able to focus on your friend's specific voice is analogous to being able to focus on only radio waves of a specific frequency)*

In the image below, we can see a red radio wave. This radio wave is actually the combination of 5 radio waves of different frequencies! In the blue, we see this same signal analyzed and split into the individual components, located at its respective frequencies. *(Analogy: 5 people talking at the same time, what your ears hear is the red. Your brain recognizing that these are 5 different voices is the blue)*
![Two graphs, top graph is a red sinusoidal signal in the time domain, bottom graph is that same signal in the frequency domain showcasing 5 peaks at frequencies of 10, 20, 30, 40, and 50Hz.](https://upload.wikimedia.org/wikipedia/commons/thumb/3/30/FFT_of_Cosine_Summation_Function.svg/440px-FFT_of_Cosine_Summation_Function.svg.png)

The mathematical process of going from the red representation (the signal in the time domain) to the blue representation (the signal in the frequency domain) is called the "Discrete Fourier Transform". *You may also hear about something called the "Fast Fourier Transform" which is the discrete fourier transform, but a very clever and fast way to calculate it.*

*(Intuitively, the DFT/FFT is essentially comparing a bunch of sine waves of varying frequencies to the signal detected, and calculating how similar they are. This would be like your brain iterating through all possible human voices and checking to see how strongly your ears hear that particular voice)*

**This means that even though there may be a jumbled mess of radio waves surrounding us at all times, if they are radio waves of different frequencies, we are able to distinguish between all these different frequencies using some old fashioned engineering.**

This non-trivial fact is what allows our world to have so many different types of devices communicating wirelessly all at the same time! They are all using radio waves, all in the same space, but at different frequencies! This is why radio waves are identified by their frequency (or wavelength).

### Summary

In this section on radio waves, we learned about what they are, the basics of how they're produced, frequency & wavelength, and how we distinguish between different radio waves co-existing in the same space. These are the fundamental concepts behind the physics of radio waves that engineers take advantage of.

In the next section on antennas, we will learn what they are and how they are used to efficiently propagate/send radio waves.

In another section, we will learn more about how exactly we manipulate radio waves to convey the information we want to send.

## Antennas

We use antennas in our everyday lives, but most people don't know how they work. We use antennas on cars, on buildings, and even within our computers and phones!

### How they work

#### Material properties 

Antennas are all made up of electrically conductive material. This usually means antennas are made out of metals like copper. 

Materials that are electrically conductive allow electrons to freely move throughout it. The opposite type of material would be electrically insulating material.

As a real-life analogy, imagine a typical swimming pool filled with water. A normal pool like this allows people to freely swim through it! In this analogy the water represents electrically conductive material and people represent electrons. Now imagine a piece of copper metal as being that pool of water. Because copper is electrically conductive, electrons can freely move through it!

An analogy for an electrically insulating material would be a swimming pool filled with jello/pudding/gelatin. If someone tried diving into that pool, they wouldn't get very far and it'd be super hard or impossible to swim through it. In this analogy the jello/pudding/gelatin represents electrically insulating material and people represent electrons. Now imagine a piece of plastic being that pool of water. Because plastic is electrically insulating, eletrons are mostly stuck where they are inside the plastic!

#### Taking advantage of moving electrons
Antennas need to be electrically conductive because they transmit and receive radio waves by taking advantage of the movement of electrons.

In the above section on "How radio waves are created" we learned that radio waves are generated by moving an electron back and forth in an oscillating (repetitive) motion. This oscillation of electrons occurs in the antennas. 

If antennas weren't electrically conductive, these electrons wouldn't be able to move and create these radio waves!

#### How transmitting with an antenna works

In order to transmit with an antenna, the antenna needs to be connected to an electrical component that can control the movement of these electrons.

TODO: expand/clarify?

#### How receiving with an antenna works

Antennas receive radio signals passively. As the electromagnetic fields are manipulated in the environment around an antenna, the electrons in the antenna move accordingly (because of how the physics of it work). By monitoring the movement of these electrons, the respective radio wave can be captured.

TODO: expand/clarify?

## Communicating with radio waves

How do we as humans harness this power of manipulating radio waves as communication? We agree on a bunch of different rules and conventions on how we will manipulating these radio waves to convey information efficiently and responsibly. In the United States, most of these rules are established and enforced by the Federal Communications Commission (FCC).

### An analogy
To lay a conceptual foundation for this relatively abstract section, here is an analogy. 

Suppose we have two friends with the simple names of "A" and "B" that want to talk to each other over a distance, but they cant see, hear, or touch each other over that distance. The only way method of communication they have is a really long piece of rope between them. So once they travel far away from each other, they will each be holding one end of the rope and will be trying to communicate.

But before they travel far away from each other, they need to talk to each other to figure out a set of rules of communication so that they can understand each other when they feel the rope moving.

When coming up with these rules for communication, one of the first things that A & B need to do is come up with a language. Because A & B really like computers, they chose the language of computers: binary. They chose this language because it is super simple and effective; it only has two letters (1 and 0) and you can send tons of information by being clever with 1s and 0s (thats what computers do).

Now the next thing that A & B needs to do is figure out how to use the rope to send these 1s and 0s. Here are some ideas they brainstormed together:
* If the rope is moving up and down (oscillating), then that is considered a 1. If the rope is not moving, then that is considered a 0.
* If the rope is oscillating super fast then that's a 1, but if the rope is oscillating slowly then that's a 0.
* If the rope is oscillating super fast then that's a 0, but if the rope is oscillating slowly then that's a 1.
* If the rope is oscillating with a big height then that's a 1, but if the rope is oscillating with small height then that's a 0.

A & B realized two properties of the movement of the rope that they could capture: (1) frequency, how fast the rope is oscillating, and (2) amplitude, how tall the rope is when its oscillating. 

The term for manipulating these properties is "modulation". This makes sense because a dictionary definition of modulation is "to adjust"; modulation is just a fancy word for changing.

In this analogy, the material/medium of communication was the rope. But in our context, the material/medium of communication is radio waves. Both of these materials have frequency and amplitude that can be modulated to send information, and demodulated to receive that information.

In the next section, we'll learn about frequency and amplitude modulation. We'll also learn about some other types of modulation that don't have a direct real-life connection to ropes.

### Modulation

#### Frequency modulation

#### Amplitude modulation

#### Other modulation schemes

### Frequency allocation

#### Baseband signal and carrier signals

#### Bandwidth

#### Bands and Channels

## Wireless Communication Protocols

In the previous section on "Communicating with Radio Waves", we learned how we are able to transmit messages wirelessly over radio waves via modulation.

We understood this through our analogy with friends A & B who came up with a way to send letters in their binary language (1 and 0) through a rope. Assuming they were able to do this successfully, they could send long streams of messages that look something like "101011100101010101010". But that's still not enough! They need to come up with a 'protocol' for deciphering what this message actually means!

For example, they could come up with a simple protocol for sending messages about how their day went. 
* The first 3 letters would represent how their day went: "101" could mean that they had a good day, and "010" could mean that they had a bad day, and maybe "110" means that their day went okay.
* The next 3 letters would represent the weather: "100" could mean that it was sunny and "101" could mean that it was rainy.

We have these same types of protocols in real life that are much more complicated. They are carefully designed to ensure security (can people intercept messages?), reliability (what if the message gets messed up in certain places?), and efficiency (how much useful data can we send at a time?).

### WiFi

### Bluetooth

### Cellular communication
