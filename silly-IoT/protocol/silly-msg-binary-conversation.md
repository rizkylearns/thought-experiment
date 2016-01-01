<!-- 
@author Rizky Farhan [rizky.farhan@gmail.com]
@license MIT [http://rizky.mit-license.org]
-->

# Silly Message (Binary) Conversation Protocol

Silly Message Protocol describes a message protocol for TCP connected devices.
A fixed order of conversation procedure for Silly Message Protocol is described below.
It should look simple and silly enough. 
A Silly Message Conversation Protocol has three states:
- Disconnected
- Connected
- Conversation

## Disconnected
This conversation state is the default state. A device is, by default, disconnected from the data server.

## Connected
Once a TCP/IP connection is established, the conversation is considered in Connected state. At this state, a device is ready to send its first compulsory LOGIN message.
A LOGIN message is required to determine whether the device messages can be persisted by the data server.

## Conversation
Once a LOGIN message is accepted and the message source ID is acknowledged, Conversation state dictates that the message source ID is accepted to both send and receive messages until the conversation state moves back to Disconnected state.

# Silly Message (Binary) Conversation Protocol Procedure
- If a device is in DISCONNECTED state, then it can stay DISCONNECTED, or it can transition to CONNECTED state
- If a device is in CONNECTED state, then it can stay CONNECTED, or it can transition to DISCONNECTED or CONVERSATION state
- If a device is in CONNECTED state, and a LOGIN message is sent from the device, then it can transition to DISCONNECTED state if LOGIN is rejected, or CONVERSATOIN if accepted
- If a device is in CONVERSATION state, then it can continue sending messages, or receive messages from the data server
- If a device is in CONVERSATION state, then it can stay in CONVERSATION, or it can transition to DISCONNECTED state.

@author Rizky Farhan [rizky.farhan@gmail.com]
@license MIT [http://rizky.mit-license.org]
