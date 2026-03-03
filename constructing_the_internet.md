# Constructing the Internet (a story)

Imagine you’re in your room with a laptop and a PC, and you want them to communicate.

The most obvious solution is also the simplest: **connect a cable between them** and send bits through it. It works. You can picture the data traveling as electrical signals from one device to the other.

But then you add a third device.

Now the old strategy becomes annoying fast: if every device must be directly connected to every other device, then **each new device requires many new connections**. Two devices need one cable. Three devices need three cables. Four devices need six cables… and the mess grows quickly.

So we step back and ask:

> “Can we build a network that scales without turning our room into a web of cables?”

## A single meeting point

Instead of connecting every device to every other device, we introduce a single central device that everyone connects to once.

- Your laptop connects to it.
- Your PC connects to it.
- Your phone connects to it (via Wi-Fi).
- Any new device only needs **one** connection: to the central point.

This central device is typically:
- a **switch** (when devices connect by cable), or
- an **access point (AP)** (when devices connect by Wi-Fi).

In many homes these come packaged together (often called a “Wi-Fi router”), but the idea is the same: **one meeting point, many devices**.

Now communication changes:

- In the old design: `A → B` (direct cable)
- In the scalable design: `A → switch/AP → B`

We didn’t choose this because it’s “more complicated.”  
We chose it because it is **maintainable** and **scales well**: adding a device doesn’t force us to redesign the entire room.

At this point, we have a **local network** (a LAN).

## The next necessity: talking to a network outside your room

Now imagine someone in another part of the city has their own local network.

- In your network (Network 1): devices **A** and **B** connect to your switch/AP.
- In their network (Network 2): devices **C** and **D** connect to their switch/AP.

You want device **A** to communicate with device **C**.

At first, you might think:

> “Let’s just connect the two central devices together.”

But there’s a problem: **switches and access points are designed to organize communication _inside_ one local network**, mainly between end devices. They don’t exist to connect separate networks that may have their own internal rules and their own internal identifiers.

So we introduce a new kind of device with a different purpose:

## A device made to connect networks: the router

A **router** is not “a better switch.” It’s a different tool for a different job.

- A **switch/AP** connects **devices inside one network**.
- A **router** connects **one network to other networks**.

This matters because once you connect networks, you face a new issue: **identifier collisions**.

### Why two kinds of addresses exist (local vs global)

Inside your local network, every device needs a unique identifier, otherwise you couldn’t reliably say “send this message to B.” So devices have **local addresses** that are unique *within that network*.

But another network elsewhere can independently assign addresses too. That means your device **A** could have a local identifier that coincidentally matches the local identifier of their device **C**. If we tried to merge everything into one giant local system, those identifiers could collide and become ambiguous.

That’s why communication across networks uses a second layer of identity:

- A **local address** (unique inside its own network)
- A **global/internet address** (unique across the network-of-networks)

So a device can keep its local identity for local traffic, while also having a globally unique identity for “outside-world” traffic.

## From two networks to a global system

Now the path from **A** to **C** looks like this:

1. **A** sends the message inside Network 1 to the local meeting point (switch/AP).
2. The message reaches the **router** (the gateway to the outside).
3. The router forwards it toward Network 2 using global addressing.
4. Eventually a router near Network 2 delivers it into that network.
5. Inside Network 2, the local meeting point forwards it to **C**.

What’s powerful here is that **each local network keeps its internal structure**.  
We don’t “break” the design we used in our room. We just add a new layer that connects networks together.

And once we can connect two networks, we can connect thousands… then millions.

That idea, **connecting networks globally while letting each one keep its identity**, is what we call the **Internet**: a network of networks.
