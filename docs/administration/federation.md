# Networking Multiple Leaf instances
One powerful feature of Leaf is the ability to federate user queries to multiple Leaf instances, even those using different data models. This enables institutions to securely compare patient populations in a de-identified fashion. An example of this functionality can be found at <a href="https://www.youtube.com/watch?v=ZuKKC7B8mHI" target="_blank">https://www.youtube.com/watch?v=ZuKKC7B8mHI</a>. 

!!! info 
    Networking with other Leaf instances is **100% opt-in** functionality. Deploying locally and querying only your institution's data is perfectly fine.

<p align="center"><img src="../images/multi_instance_no_header.png"/></p>

### Building Blocks
Leaf achieves safe federation by leveraging existing web technology to establish cryptographic trust between instances. This mechanism is virtually identical to the way that SAML2 federated authentication establishes trust and is built on JSON Web Tokens (JWT). The core building block of federated communication is a uni-directional channel; composing two uni-directional channels together provides the bi-directional query functionality of mutually federated instances. Federation is opt-in and requires that two instances be in agreement about their relationship. A single endpoint can be configured in either, or both, of the following states:

#### Responder
A `Responder` endpoint is an instance of Leaf's API that will answer query requests, provided that it has configured your instance as an `Interrogator`.

#### Interrogator
An `Interrogator` endpoint is an instance of Leaf's API that will be allowed to send query requests to your API instance. The federated `Responder` in turn must have your instance configured as a `Responder` for requests to come through.

### Cross-pollinating API instance certificates
1. Log into a Leaf admin account and navigate to `Admin` -> `Network and Identity`.
2. Click `+ Add New Networked Leaf Instance`.
3. Enter the name and URI for the instance you wish to federate with.
4. Click `Load Certificate Information`.
5. Configure the endpoint's `Responder` (we can query them) and `Interrogator` (they can query us) roles, see below for examples.
6. Click `Save`.


### Examples
#### Bi-directional Federation
In this (most common) example, two nodes A and B wish to query each other. This configuration requires that A registers B as a `Responder` and `Interrogator`, and B registers A as a `Responder` and `Interrogator`. The following screenshots illustrate A and B's admin panel screen, respectively.

=== "Instance A's networking configuration"

    <p align="center"><img src="../images/fed_mutual_a.png"/></p>

=== "Instance B's networking configuration"

    <p align="center"><img src="../images/fed_mutual_b.png"/></p>


#### Uni-directional Federation
In this example, there are two nodes X and Y where X would like to additionally query Y for data but Y has no need to query X. X registers Y as a `Responder`, and Y must register X as an `Interrogator`. This type of configuration can be composed with `Gateway` functionality in cases where an instance wishes to act as an observer only and has no data of its own to offer. The following screenshots illustrate X and Y's admin panel screen, respectively.

=== "Instance X's networking configuration"

    <p align="center"><img src="../images/fed_uni_x.png"/></p>

=== "Instance Y's networking configuration"

    <p align="center"><img src="../images/fed_uni_y.png"/></p>
