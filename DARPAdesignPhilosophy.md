# The Design Philosophy of the DARPA Internet Protocols
  - using the 3 pass approach

## 1st pass
  - just going to read the pieces and not take any notes during the first pass.

## 2nd pass: notes

#### Fundamental Goal

  - TCP and IP are fundamental layers to the design of todays internet, but weren't in the original proposal.
    - they were integrated due to testing before the final draft of the proposal.

  - The original goal of DARPAs research was to develop a technique to  utilize all the existing/interconnected networks that already existed.
    - the very fist networks that were to be connected were ARPANET and the ARPA packet radio network

  - One interesting problem is that each network represented (and still does today) an independent domain of control.
    - i.e. each network is administrated by the org that controls that network.
    - a goal of the original internet was to tackle this issue
  - Networks are connected to each other via gateways
    - gateways are layers of internet packet switches
    - gateways use a packet switching algorithm called *store and forward*

#### Second Level Goals
  - The ordering of the 7 goals of the internet highly influenced its design
    - They are ordered from most to least important
  - Since the original context for the internet project was military, the survival of the network was placed higher than its accountability

#### Survivability
  - The main goal: create an architecture where the only way to lose all communication is if there is no physical path over which ANY sort of communication can be achieved
  - So this means that downstream layers must have state persistence.
    - This includes # of packets transmitted, # of packets acknowledged, or the # of outstanding flow control permissions
    - so one way to do this is to have intermediate layers in the network save the state of transmissions.
  - The way that was chosen was the 'dumb pipe'. The intermediate layers know nothing of the state of packets travelling across them.
    - Instead the client and endpoints save the state.
  - **fate sharing**: so the state of a transmission is only lost if both the endpoint and the entity it communicates with are lost - or lose their state.
    - There are two important advantages of fate sharing over replication (meaning replicating state in the intermediate layers):
      1. FS protects against any number of intermediate layer failures.
      2. FS is much easier to engineer than replication
    - There are two major consequences of FS
      1. Intermediate layers (packet switching nodes/gateways) cannot have any state information. This class of network design is called "datagram network"
      2. There is no reliable delivery of data that is guaranteed. All the responsibility of state information is on the host and entity

#### Types of Service
  - The traditional type of service is called *virtual circuit* service
    - this is bi directional reliable delivery of data
    - it was the first type of service provided by TCP
    - examples are login services and file transfer

  - TCP was originally designed to be general enough to support any needed type of service.
    - this didn't work out
    - XNET and digitized speech were examples of services that didnt fit with TCP
    - digitized speech needed to have guaranteed order of packets

  - because TCP was obviously unable to handle all types of needed services, it was decided that more than one transport protocol would be required

  - The minimum requirements of the new protocols is that they can simultaneously constrain the following 3 things:
    1. reliability
    2. delay
    3. bandwidth

  - This caused TCP and IP to be split (they were originally all wrapped under TCP)
    - TCP provided the reliability requirement
    - IP allowed for variety of services

  - UDP stands for *User Datagram Protocol*

# Questions for test - self answers

1. Discuss the advantages of the fate-sharing survivability model present in Internet
Protocols today, and how this model differs from a distributed state/replicated
survivability model.
  - There are two direct advantages of FS discussed in the paper.
  - The first is that FS protects against any number of failures in intermediate layers of the network.
    - This differs from replication in that intermediate layers in a distributed state model must detect errors and inform down/up stream layers (including the host and entity)
  - The second is that FS is much easier to engineer than replication.
  - One consequence is that there is no guaranteed reliable delivery in a datagram network (FS network).

2. Explain how the Internet architecture achieves the flexibility required to support a wide
variety of networks? What functions is the network assumed to provide, and more
importantly, what functions of the network are NOT assumed?

3. How has the ARPANET design goal of distributed resource management across the
network not been met in today’s Internet?

4. Describe two of the design advantages of datagrams as the basic architectural feature of
the Internet
