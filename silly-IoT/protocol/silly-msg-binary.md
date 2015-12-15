<!-- 
@author Rizky Farhan [rizky.farhan@gmail.com]
@license MIT [http://rizky.mit-license.org]
-->

# Silly Message (Binary) Protocol

A Silly Message Binary Protocol (SMBP) is composed of three fields:
- Header : Prefix data to explain the rest of the data
- Body   : a dynamic sized body
- CRC32  : A 32-bit error correction field

## Endianness ##

I always face a tough time since college to determine the endianness of anything.
So this protocol uses Big-Endian byte order notation, since Network Byte Order 
is Big-Endian. 
So a hex value 0xFFAACCEE is a big endian byte order notation
where it could be stored in a little-endian memory registry as 
[ 0xEE 0xCC 0xAA 0xFF ], where [] is a memory where the left-hand-side is 
the lowest address (where the lowest significant byte is stored), and 
the right-hand-side is the highest address (where the most significant byte is 
stored). In this article, however, I will still show 0xFFAACCEE..

## Silly Header ##

A Silly Header consists in a single SMBP consists of the following fields
in sequence:

1. [ 1 Byte ] Encoding. For SMBP, this field always returns [ 0x00 ].
2. [ 1 Byte ] Message Type: Single-GPS [ 0x01 ], Multi [ 0x02 ] or other types 
(not yet determined). For SMBP, this field always returns [ 0x01 ]. 
This Message Type determines how the scheme for the rest of the field looks 
like (size of msg size, size of crc, body schema, etc). 
Since SMBP always returns [ 0x01 ], The next sequences only deals with 
message Type 'Single-GPS'.
3. [ 4 Byte ] Body Size. Not including CRC bytes. Not including previous bytes.
Not including itself (Body Size). Includes System Timestamp. Includes topic.
Includes 0 to N Byte data.
4. [ 8 Byte ] Source ID. This is an 8-byte sized field. This is a 64-bit number, compulsory to be sent in every message to maintain the statelessness of the message identification. One example of having this message source id is to dedicate a storage for each message source id.

### Silly Header Remarks ###

I don't know how long is the lifetime of this document. I only know that it is 
a good practice for me to design a message protocol that it can be opened for 
extension.

## Silly Body ##

5. [ 8 Byte ] System Timestamp as POSIX time (in milliseconds)
6. [ N Byte ] 0 to N Byte data payload

### Silly Body Parts ###

The SMBP Silly Body parts consists of a system timestamp (timestamp from message 
creator) and data payload. I noticed over the years of machine integration that 
'timeliness' is the utmost important piece of information. Even if there is no 
data payload, but there is a Timestamp, then a piece of data is still usable.
This is why the fields of system timestamp and payload are separated.
SMBP Silly Body parts schema is a simple trio: part field size, part field type, 
and part field value i.e. SMBP Silly Body parts data payload consists of a chain 
of 0 or more of the following:
- { 2 Byte Field Byte Size N. Not including Field Type Byte Size }
- { 1 Byte Field Type }
- { N Byte Field Value }.

So, for example (in network byte order): [ 0x000800000000005661F524 ] 
represents 8 Byte field data with type 0x00 (GPS Timestamp since EPOCH) 
and value 0x000000005661F524 or 
ISO8601 2015-12-04T20:18:44Z.
#### Silly Body Parts - The Essentials ####
- Topic
SMBP Topic is 2-Byte sized field value.
SMBP Topic field type is 0x01
SMBP Topic field value determines the payload contents
#### Silly Body Parts - The GPS data fields ####
- GPS Timestamp
SMBP GPS Timestamp is 8-Byte sized field value.
SMBP GPS Timestamp field type is 0x02
- GPS Latitude In 5 Decimal Points
SMBP Latitude is 8-Byte sized field value.
SMBP Latitude field type is 0x03
SMBP Latitude field value represents a decimal value to be resolved by 
multiplying the incoming value with 10^-5
- GPS Longitude In 5 Decimal Points
SMBP Longitude is 8-Byte sized field value.
SMBP Longitude field type is 0x04
SMBP Longitude field value represents a decimal value to be resolved by 
multiplying the incoming value with 10^-5
- GPS Heading In 2 Decimal Points
SMBP Heading is 2-Byte sized field value.
SMBP Heading field type is 0x05
SMBP Heading field value represents heading in degrees from 000 to 36000, so 
the actual incoming heading value is to be resolved by multiplying it with 
10^-2
- GPS Altitude In Decimal SMBP Altitude is 4-Byte sized field value
SMBP Altitude field type is 0x06
SMBP Altitude value is a pressure altittude in Metres from Mean Sea Level (MSL)

## CRC-32 ECC ##

7. [ 4 Byte ] CRC-32 Cyclic Redundancy Check bytes for error correction

# Silly Message Binary Protocol Field Mappings
## (Header) Encoding ##

| Value | Remarks 		   | 
|-------|------------------|
| 0x00  |  Binary Encoding |

## (Header) Message Type ##
Message Type field acts as namespace and scheming for field data.

| Value |  Remarks 		  														    | 
|-------|---------------------------------------------------------------------------|
| 0x01  |  Single-GPS. This message type is used for sending GPS sensor data scheme |

## (Body) Message Type (0x01) Field Mapping ##
Message Type field acts as namespace and scheming for field data.

### (Body) Message Type (0x01) Field Mapping: Topic
Each topic determines the machine's state at the time of message creation, exposing machine data as the machine state's attribute values at that time, if any.

| Value |  Remarks 		                                   				          | 
|-------|-------------------------------------------------------------------------|
| 0x00  |  Login. This message is a login message           			          |
| 0x01  |  GPS Report. This message contains silly Geodata of a sender machine    |



# Remarks
- This is a thought exercise for how I might want to create my own IoT Binary
Message protocol.I based my design on prior knowledge in integrating machine 
messages and patterns I noticed when reading protocol documents. 

@author Rizky Farhan [rizky.farhan@gmail.com]
@license MIT [http://rizky.mit-license.org]
