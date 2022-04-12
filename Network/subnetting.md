***IPv4 address*** is a 32 bit numeric address written as 4 numbers seperated by dots. Each group of numbers are called octet. Numbers in each octet can be from 0 to 255.
An IP address consists of two parts: Network address and Host address. Network id or network address is a number that is assigned to a network.
Every network has a unique address.Host address or or host id is what is assigned to a host(device) within that network.

Subnet mask can be used to tell which part of the IP address is network or host address. A ***subnet mask*** is the number that resembles the IP address.
A ***subnet mask*** reveals how many bits in the IP address are used for the network by masking the network portion of the IP address.
The computers and networks only understand binary format. How to get the binary address from an IP address or a ***subnet mask***?
Below is the process starting with 8 bit octet chart.

**8 Bit Octet Chart**
|  2<sup>7</sup>  | 2<sup>6</sup>   | 2<sup>5</sup>   | 2<sup>4</sup>   | 2<sup>3</sup>  | 2<sup>2</sup>  | 2<sup>1</sup>  | 2<sup>0</sup> |
| -- | -- | -- | -- | - | - | - | -|
|128 | 64 | 32 | 16 | 8 | 4 | 2 | 1|

IP address 
`192.168.1.0`   -->  `11000000.10101000.00000001.00000000`

Subnet mask
`255.255.255.0` -->  `11111111.11111111.11111111.00000000`

When the subnet mask binary digit is 1 it will indicate the position of the IP address that defines the network.
Example:
    
    11000000.10101000.00000001.00000000
    |||||||| |||||||| ||||||||
    11111111.11111111.11111111.00000000
     NETWORK NETWORK  NETWORK   HOST
     192    .168     .1        .0

Another example for IP address `10.0.1.0` and Subnet mask `255.0.0.0`:
    
    00001010.00000000.00000001.00000000
    ||||||||
    11111111.00000000.00000000.00000000
    NETWORK    HOST    HOST      HOST
    10      .0       .1       .0

Why does and IP address have a network and a host part?
The reason is  managibility. It's for breaking larger networks into smaller networks or into subnets.

Subnetting is done by changing the default subnet mask by borrowing some the bits from the host portion.

TBC
