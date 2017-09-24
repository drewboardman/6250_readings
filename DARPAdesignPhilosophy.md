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
  - The main goal: create an architecture where the only way to lose all communication is if there is no physical path over which ANY sort of
    communication can be achieved
  - So this means that downstream layers must have state persistence.
    - This includes # of packets transmitted, # of packets acknowledged, or the # of outstanding flow control permissions
    - so one way to do this is to have intermediate layers in the network save the state of transmissions.
  - The way that was chosen was the 'dumb pipe'. The intermediate layers know nothing of the state of packets travelling across them.
    - Instead the client and endpoints save the state.
  - **fate sharing**: so the state of a transmission is only lost if both the endpoint and the entity it communicates with are lost - or lose their
    state.
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

  - The minimum requirements of the new protocols is that they can simultaneously constrain the following 3 things: 1. reliability 2. delay 3.
    bandwidth

  - This caused TCP and IP to be split (they were originally all wrapped under TCP)
    - TCP provided the reliability requirement
    - IP allowed for variety of services

  - UDP stands for *User Datagram Protocol*
  - One issue with the new datagram model was that it required the intermediary networks to be agnostic of the information or service the datagram was
    being used to transmit.
    - For instance, the X.25 network had built-in delays to ensure reliable delivery (which is not a requirement or desire of the datagram model).
      There is no way to turn this feature off in an X.25 network.

#### Varieties of Networks
  - a number of services are *explicitly* **not** assumed about the network
    1. reliable or sequenced delivery
    2. network level broadcast or multicast
    3. ranking packets by their priority
    4. support for multiple types of services
    5. knowledge of the e2e speed or failures along the transmission

#### Other Goals
  - inefficiency is a problem with the multiple networks model. Here are some examples:
    1. packet headers are (on average) 40 bytes. So a packet with 1 byte of data has a 40x header overhead. Whereas a packet with 1kilobyte of date has
      only about a 4% overhead
    2. another source of inefficiency is retransmission
      - since the intermediate networks do not maintain any state information, the retransmission of lost packets has to travel the entire length of
        the network a second time
    3. The cost of connection a host to the internet is higher than if the intermediate networks implemented all of the service protocols
      - developers now need to implement all the necessary agreements on their endpoints

#### Architecture

#### Datagrams
  - this is the fundamental architectural feature of the internet
  - There are several reasons that it is important to use packets/datagrams
    1. They remove the need to maintain connection state within the intermediate networks
      - This also means that hosts/clients can recover after connection loss
    2. It allows a variety of different types of services to be implemented
      - as long as they all use datagrams
    3. It assumes the least possible about the network
      - this allows a large variety of networks to be used

### TCP
  - flow control is based **only on the # of bytes**.
    - It originally was both bytes and packets, but that was overly complex
  - one advantage of the byte control vs packet control is that a ton of 1 byte packets can be aggregated into 1 larger packet that is more sane to be
    transported across the network

# Questions for test - self answers

1. Discuss the advantages of the fate-sharing survivability model present in Internet Protocols today, and how this model differs from a distributed
state/replicated survivability model.
  - There are two direct advantages of FS discussed in the paper.
  - The first is that FS protects against any number of failures in intermediate layers of the network.
    - This differs from replication in that intermediate layers in a distributed state model must detect errors and inform down/up stream layers
      (including the host and entity)
  - The second is that FS is much easier to engineer than replication.
  - One consequence is that there is no guaranteed reliable delivery in a datagram network (FS network).

2. Explain how the Internet architecture achieves the flexibility required to support a wide variety of networks? What functions is the network
assumed to provide, and more importantly, what functions of the network are NOT assumed?
  - The network felixibility was acheived by using the datagram model. Particularly, all of the requirements are implemented by the host that is
    connecting to the network. The intermediate network behaves as a 'dumb pipe', meaning that there is no state information that persists in the
intermediate network. That is all handle by the host and client.
  - The functions that are **not** assumed are under the "Varieties of Networks" portion
    - They are: reliability or sequenced (in order) delivery. Network level broadcast. Prioritizing packets. Support for multiple types of service.
      Knowledge of total network speed or intermediate failures.

3. How has the ARPANET design goal of distributed resource management across the network not been met in today’s Internet?

4. Describe two of the design advantages of datagrams as the basic architectural feature of the Internet
