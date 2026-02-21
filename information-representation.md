# Text Representation

When I first understood what a computer *really* stores, it stopped feeling like magic.

A computing system can store electricity in a controlled way thanks to circuits like **latches** and **flip-flops**. Each latch is like a tiny memory spot that can reliably hold one of two states:

- **0** (off)
- **1** (on)

I like to imagine them as **bulbs**. A bulb can be off or on, and that’s a **bit**.

## The 8-bulb cell (a byte)

Now imagine we group 8 of those bulbs into a single “cell”.

That gives us **8 bits**, also known as **1 byte**.

Because each bulb can be either 0 or 1, the number of possible patterns is:

- 2 choices per bulb
- 8 bulbs

So we get:

- **2⁸ = 256 combinations**

That means one byte can represent any number from **0 to 255**, depending on the pattern of bulbs.

Example patterns:

- `00000000` (all off)
- `00000001` (only the last bulb on)
- `11111111` (all on)

At this point, notice something important:

> The computer doesn’t know what these patterns *mean*.  
> It only knows how to store and move them.

Meaning comes from *us*.

## Meaning is a human agreement

A byte like `01000001` is just a pattern of electricity.

It could mean:
- the number **65**
- a color intensity for a pixel
- part of an instruction for the CPU
- or a letter

So the real trick is:

> We **agree** on a mapping between byte patterns and symbols.

And that’s exactly why standards like **ASCII** and **UTF-8** exist.

## ASCII: a shared map from bytes to characters

**ASCII** is one of the earliest and most important agreements.

It defines a table: a mapping that says which numbers correspond to which characters.

For example:

- `65` → `"A"`
- `97` → `"a"`

Historically, ASCII was designed as a **7-bit** system, meaning it defined **128 characters** (0–127).  
But since computers commonly store data in **8-bit bytes**, ASCII text is typically stored inside bytes anyway, using the same familiar 0/1 patterns.

ASCII is why text could be shared across different machines without everyone inventing their own private “letter codes”.

## The limitation: ASCII is too small for the world

ASCII works well if your world is basically:
- English letters
- digits
- punctuation
- a few control codes

But the moment you want:
- `ñ`, `á`, `ç`
- `汉字`
- `🙂`
- Arabic, Greek, Hindi, and thousands of other symbols…

ASCII runs out of space.

So the next idea was:

> Let’s create a universal list of characters for *all languages and symbols*.

That’s **Unicode**: a huge catalog where every character gets a unique number.

But Unicode alone doesn’t say how to store those numbers in bytes.

That’s where **UTF-8** enters the story.

## UTF-8: a smart way to store Unicode using bytes

UTF-8 is an encoding—rules for turning Unicode characters into bytes.

Its genius move is:

1. **It keeps ASCII exactly the same** for the first 128 characters.
   - So any plain ASCII text is automatically valid UTF-8.
2. For characters beyond ASCII, it uses **multiple bytes**.
   - Some characters take 2 bytes
   - others take 3 bytes
   - and some take 4 bytes

So in UTF-8:

- `"A"` uses **1 byte**
- `"ñ"` uses **2 bytes**
- `"汉"` uses **3 bytes**
- `"🙂"` uses **4 bytes**

## A small correction to a very natural idea

At first it feels natural to say:

> “One letter equals one byte.”

That’s often true in ASCII (and in UTF-8 for plain English letters), but it’s not always true in UTF-8.

So if you have 2 memory cells (2 bytes):

- You can store **2 ASCII characters** (because each uses 1 byte)
- Or you might store **only 1 character** like `"ñ"` (because it uses 2 bytes)
- And you *cannot* store a 3-byte character like `"汉"` with only 2 bytes

The memory is still just bulbs—bits—but the number of characters you can store depends on the encoding and the characters you choose.

## The key mental model

This is the clean way to think about text:

**bits (electric states) → bytes (groups of bits) → encoding rules → characters (human meaning)**

ASCII and UTF-8 are not “the text itself”.

They are **agreements**—rulebooks that let different computers interpret the same stored patterns in the same way.

And once you see that, text stops being mysterious:

It’s just electricity… with a shared dictionary on top.

---

# Number Representation

After understanding how text works (ASCII / UTF-8), I ran into a new confusion:

Why does number representation look *so different*?

The key is that the computer is still storing the same thing as always: **bits** (bulbs on/off).
What changes is the **meaning we assign to those bits**.

## Two ways to store the “same” thing

Let’s take the number **1024**.

### 1) Storing it as text (characters)

If I store `"1024"` as **text**, I’m not storing the number 1024 directly.

I’m storing four characters:

- `'1'`
- `'0'`
- `'2'`
- `'4'`

In ASCII/UTF-8, each of those characters typically takes **1 byte** (8 bits).

So `"1024"` as text costs:

- 4 characters × 8 bits each = **32 bits**

And notice what the computer “sees”:
It doesn’t see *one number*.  
It sees **four separate symbols**, each with its own code.

### 2) Storing it as a number (binary value)

But if my goal is to store the numeric value **one thousand twenty-four**, there’s a more direct approach:

Store the **value itself** in binary.

A binary number uses bits as place values:

- with 1 bit you can represent 2 values
- with 2 bits you represent 4 values
- with 10 bits you represent 2¹⁰ = 1024 values (from 0 to 1023)
- and to represent the value **1024** specifically, you need **11 bits** (because 1024 is 2¹⁰)

So the important correction is:

> 2¹⁰ = 1024 is the *count of values you can represent with 10 bits* (0 to 1023).  
> To store the number 1024 itself, you need **11 bits**.

Still, the big point remains true:

Storing a number directly in binary is far more compact than storing its digits as characters.

## Why text is “inefficient” for numbers

Text storage is great when your main goal is:
- to display the number to humans,
- to send it through text-based protocols,
- or to write it into a readable file.

But for computation and memory efficiency, it’s wasteful because:

- `"1024"` uses **4 bytes** (at least)
- while the numeric value 1024 fits in **far fewer bits** (11 bits)

So we’re comparing:

- **Text representation**: stores symbols `'1''0''2''4'`
- **Numeric representation**: stores the value as a binary magnitude

Same *meaning for us*, very different *meaning for the machine*.

## The deeper lesson

This connects back to the earlier idea:

> Bits have no meaning by themselves.  
> Meaning depends on interpretation.

The exact same byte pattern could be:
- the text `"A"`
- the number 65
- part of an instruction
- a pixel color

So when someone says “how many bits does 1024 use?”, you always have to ask:

**Do you mean the text `"1024"`… or the numeric value 1024?**

---

# Image Representation

After text and numbers, images felt like the next big jump.

With text, it’s easy to imagine: a byte pattern becomes a letter because we agree on a table (ASCII / UTF-8). With numbers, the bits *are* the value.

But an image is different. An image isn’t a single symbol or a single value.

An image is a **whole arrangement** of many tiny points in space.

So I needed a new mental picture.

## The screen as a grid of bulbs

A very early and powerful idea is this:

> Imagine the screen as a grid of bulbs.

At the beginning, every bulb is off, so the screen is dark.

Now suppose I create a memory storage that matches the screen:

- one memory cell per bulb
- and each cell stores just **1 bit**: `0` or `1`

Then I can decide what appears:

- `0` → that bulb stays off  
- `1` → that bulb lights up

So if I store a `1` in the cell that corresponds to a specific position, that position lights up.

This is the simplest possible image system:

- it’s basically **black and white**
- or more precisely: **off/on**

And yet, it’s enough to draw shapes, letters, patterns… even simple animations if you change the bits over time.

This is the idea behind thinking of image memory as a **map**:

> a region of memory where each stored bit controls a point on the screen.

## The limitation: only two “colors”

But this bulb screen has a hard limitation:

A bit only gives two choices.

So you don’t get real color. You only get:
- dark vs light
- background vs foreground

To go beyond that, each “bulb” needs more than just on/off.

It needs a way to represent **different colors and intensities**.

## Pixels: each tiny cell becomes a color unit

This is where the idea of a **pixel** becomes important.

Instead of thinking “one bulb that is either off or on”, we think:

> each cell on the screen is a pixel that can take many colors.

And to do that, we store *more bits per pixel*.

## RGB: building color from three channels

A common model is **RGB**:

- **R** = Red intensity
- **G** = Green intensity
- **B** = Blue intensity

Each channel is usually stored in **1 byte**:

- 0 = none of that color
- 255 = maximum intensity of that color

So one pixel often takes:

- 1 byte for Red
- 1 byte for Green
- 1 byte for Blue

That’s **3 bytes per pixel** (24 bits).

And now the “screen memory” becomes:

> a big block of memory where each pixel’s position maps to three stored bytes (R, G, B).

## From bitmaps to real images

So we have a progression:

### 1-bit images (monochrome)
- 1 bit per pixel
- off/on
- very compact
- very limited

### 24-bit images (RGB)
- 24 bits per pixel (3 bytes)
- millions of colors
- heavier storage, but far richer images

## The same lesson keeps repeating

Just like with text and numbers:

> The computer is still only storing patterns of bits.

What changes is the interpretation:

- in text, bits become characters
- in numbers, bits become values
- in images, bits become **a grid of pixels**, and each pixel becomes **color information**

It’s not that the computer “stores an image” like we see it.

It stores a *recipe* — a structured chunk of memory — that a display system can translate into light.

---

## Sound Representation

What made sound feel different from text or images is that sound is not “a thing sitting still”.
Sound is a **process**: a wave that changes over time.

### Sound is air pressure changing
In the real world, sound is a wave traveling through air (or any medium).  
Air particles push and pull on each other, creating regions of higher and lower pressure that move forward.

When we say a sound is “louder”, what we usually mean is:

- the pressure changes are **bigger**
- the wave has **greater amplitude**

### The computer stores measurements, not the wave itself
A sound wave in real life is **analog**: it can change smoothly and continuously.

But a computer stores discrete values (bits), so it can’t store the wave as a perfect continuous curve.

So it does something very practical:

> It measures the wave’s amplitude at regular moments in time, and stores those measurements as numbers.

You can think of it like taking a “snapshot” of the wave again and again.

This is called **sampling**.

### Sampling rate: how many measurements per second
We choose how often we measure the wave.  
That choice is the **sample rate**.

- 8000 samples per second means: “measure the amplitude 8000 times every second”
- that’s one measurement every 1/8000 of a second

For human speech, 8000 samples per second can be enough for the voice to be understandable and recognizable.  
It’s a practical compromise between quality and storage/transmission cost (which is why telephones used it for years).

### More samples per second captures more detail
The more frequently we measure:

- the more accurately we capture fast changes in the wave
- and the closer the reconstructed sound will be to the original

That’s why music systems tend to use higher sample rates (like 44,100 samples per second for CDs): music contains more detail that we can notice.

### A small but important detail: it’s not only “how many measurements”
There are *two* dials that control quality:

1. **How often we measure** (sample rate)
2. **How precise each measurement is** (how many bits we use per sample)

So quality improves not only by measuring more times per second, but also by storing each measurement with more precision.

In short: sound storage is like turning a moving wave into a long list of numbers—one number per instant—so the computer can later rebuild a wave from that list and push it back out through a speaker.
