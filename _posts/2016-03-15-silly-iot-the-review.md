---
layout: post
title:  "Silly IoT - The Review"
date:   2016-03-16 00:00:00 +04:00
categories: IoT Career-Buildup
tags: idea concept review
---

# The Story So Far

I have implemented my Silly Message Binary Protocol (SMBP) in node-js in my [Hydra Project Github Page](https://github.com/rizkylearns/project-hydra) as a testable module. 

My test module is a TCP Net client application that simulates a GPS position running in circle around where I live in Dubai.

The client application creates an SMBP JSON Object, serializes it into a binary socket stream, and sends it to my TCP Net Server application that uses an SMBP driver to deserialize the binary stream buffer back into an SMBP JSON Object.

I made the testing client application to send the test message every 200ms. So far it did not break a sweat.

# What Did I Do Wrong?

My first mistake is the protocol name. It sounded cool at first, but now it's just, well, silly. I do like the word Silly, but no so much the rest of the words, especially the "Binary Protocol". SMBP sounds similar to SMPP, which is Short Message Peer-to-Peer, a message protocol for our every day SMS'es from our phones. 

It needs a better "Silly Protocol" name. Maybe I'll just use "Silly Protocol". Then, what about SiMP: Silly Message Protocol? Or just plain "Silly"? How about "SillyMe: A (Silly Me)ssage Protocol".

# What Else Did I Do Wrong?

The next mistake I noticed is out of SMBP module implementation. The message schema (SMBP.json) mixes up the protocol definition for Presentation and Application communication layer model. The Presentation layer should not detail the serialization/deserialization contents, as in, how to consume a data packet set and make a valid SillyMe message source set. So instead of the mixup, the presentation layer will encapsulate SillyMe message boundaries and segregate a byte stream into a contiguous SillyMe message source set.

The Application layer model, on the other hand, has to take each segregated byte array taken from the output of te Presentation layer model and produce a SillyMe message object.

## So what does the Presentation Layer require?
- a source id; Identifying a message source is extremely important to initiate a sanboxed session (conversation). Further sandboxing can be done in the persistence layer, but in SillyMe Persentation Layer, it doesn't care.
- a message schema version number; This number is the first determinant of how I can read the rest of the message. A version 1 message can follow a specific read/write message scheme, while a version 2 message can follow a different/slightly different read/write message scheme.
- a message payload size; How far ahead should I read my payload for?
- a message payload byte offset; Given the payload size, from which index do I begin my payload data in the message?
- a message crc; A fixed byte-length redundancy check to validate if I'm reading a complete message or not. If the crc validation fails, then I have to discard the message and signal my source that I should receive a full message next.

## Then what does the Application Layer require?
- a message timestamp; Timeliness is an absolute essential for an IoT message, which make it also an absolute essential for SillyMe. Providing a message timestamp gives a certainty in message order, which is a must for any kind of message processing. 
- a message originator identifier; This number is the second determinant for how I can read the rest of the message. This identifier imposes limitation on what can SillyMe driver support where only certain originator identity (as per last SMBP schema, a user or a machine), can be valid and its payload contents parseable.
- a message topic; This number is the third determinant of how I can read my message. This identifier dictates what my SillyMe driver can expect from parsing the message payload

# Is There Anything Else That's Wrong?
 
Kent Beck's TDD: Make It Work, Make It Right. Making the implementation of SMBP to a more correct SillyMe module requires a clearer implementation of ReadableStream, WriteableStream, and Stream Transform. This implementation fix indicates that I found the last implementation to be rather messy in terms of:
- Single Purposeness of the module
- Pipeable Input
- Pipeable Output

Keep in mind that my logging implementation may need another revisit to ensure that I will have my function logging to work.

# What's Next?

Next iteration will begin by visiting project-hydra/smbp and push it to a separate github repo called SillyMe to a branch called 'nodejs-impl'. Then, fix what I have found to be wrong and solidify this module. With nodejs implementation, I am going to put myself to nodeschool to spend some time to learn functional programming with javascript.