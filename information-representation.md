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
