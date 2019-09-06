# Contributing

This draft is an adopted work item of the IRTF proposed Quantum Internet
Research Group (QIRG). All discussions on this draft happen on the IRTF mailing
list. Anyone can sign up and contribute to the [mailing
list](https://www.irtf.org/mailman/listinfo/qirg). The mailing list archive is
available [here](https://mailarchive.ietf.org/arch/browse/qirg/).

## Discussions and Issues

Public issues on this repository are closed as all discussions should be held
on the QIRG mailing list. As IETF/IRTF policies require the mailing list must
remain a valid venue for contributions it is better to have one source of truth
rather than risking discussions on the same topic in multiple places. To
contribute it is best if you join the mailing list and participate in
discussions by e-mail. A list of relevant links to the mail archive is in the
[README](README.md) file.

## Pull Requests

Pull requests are open and anyone is allowed to submit. Simple editorial fixes
will likely be accepted without much discussion. Small factual corrections,
especially if they are backed up by a reference will also likely be accepted
without further discussion. Extensive changes or changes that deserve a
discussion should be accompanied by a discussion on the mailing list.

To submit a pull request, please edit the xml copy and use
[`xml2rfc`](https://xml2rfc.tools.ietf.org/) to generate the txt file. Note
that `xml2rfc` is available from Debian and Ubuntu repositories.

## List of outstanding issues and questions

Here is a list of questions and issues raised by various people who have read
and studied the draft. Since the public issues are closed, if you would like to
pick up a question and contribute, please send an e-mail to the mailing list
with an appropriate subject.

There are no rules on how to contribute, but options include:
* Individually write and submit the changes to the mailing list.
* Find a few other interested people on the mailing list, work in a small group
  on the chosen topic, and submit the final changes to the mailing list.

Note that it is not clear if this draft is the right place to tackle all of
these issues. It is also part of the discussion to answer that question.

### Major contributions

Major contributions involve adding significant amounts of content (e.g. an
entire section).

#### Definition of different qubits based on their function

The term qubit can be in itself is ambiguous. A qubit can be either the
physical realisation capable of holding a quantum state (e.g. storage qubit),
but it can also mean the actual quantum state regardless of the physical
realisation (e.g. data qubit).

It would be helpful to adopt some terms and define them explicitly in the draft
to help in that. Here are some proposed definitions:

1. Data qubit - unknown quantum state that is passed to the network to act on
   (e.g. perform teleportation)
2. Entangled qubit - qubit that is part of some Bell Pair
3. End-to-end entangled qubit - qubit that is part of a Bell pair that has been
   distributed to the two end-points that requested the Bell Pair and is either
   ready to be delivered or already has been delivered to the application
4. Storage qubits - qubits capable of temporarily storing a quantum state
5. Communication qubits - qubits that can be used to interface with inter-node
   links to generate Bell Pairs
6. Flying qubits - qubits in transit over a link

The questions are - is this list correctly defined, should there be more
definitions added, should some be removed? Should such a list be part of the
draft?

This set of definitions MUST be based on nomenclature that already exists in
the literature. It would be useful to generate such a list backed up by
references to the correct literature.

#### Data model for how entangled qubits are created, moved, destroyed

Clarify the lifetime and mechanisms involved in a qubits journey from an
unentangled initial state through being entangled with a remote qubit to
measurement which destroys the entanglement.

This needs either a section of its own or be woven into the general text
somehow.

#### Packet format

A natural question to ask is what is the packet format for qubits. There are at
least two possibilities:

1. All communication on the quantum channel always comes accompanied with a
   classical control packet, "the header"
2. There is no one-to-one relationship between qubits on the wire and control
   messages. The control messages install rules that act on entangled pairs it
   receives from the data link. In this case - is there some structure that can
   be assigned to these control messages. One could argue that the data link
   qubit is the packet.
   
The second option is to me (Wojciech) more appropriate as that way one can use
a control protocol to install relevant rules which act upon specific Bell Pairs
delivered by the data link. In this case the data link must also provide some
form of identifiers. The definition of identifiers and how they are provided is
best left for the separate work on the data link layer (also happening in QIRG,
see
[draft-dahlberg-ll-quantum](https://datatracker.ietf.org/doc/draft-dahlberg-ll-quantum/)).

It would be good to include a statement or section on whether there is such a
thing as a Quantum Packet and whether it's up to the implementation to define
this. This could tie in to the question below.

#### Definition and relation of different planes

The boundary between the data and control planes in a quantum network is not
clear at all (section above is an example of the difficulties involved). It is
clear from other works (such as the link-layer draft) that classical control
information must accompany the qubits used in the generation of the entangled
pairs. However, would these control packets be part of the data plane? Should
they be in the control plane? How are the two data planes (quantum and
classical) managed by a single control plane?

If somebody is capable of leading a discussion on this question then a section
could be added on this topic.

#### Relation to classical networks

The draft already makes the relation to classical channels clear and that being
able to communicate classically is a requirement. However, it does not make any
attempt to state where a quantum network would sit in relation to an entire
classical network. The draft also makes no mention about how this interaction
could happen. For example, should it come with its own overlay? Is it up to the
operator? Are there some general rules it must follow? What addressing does it
use? Would other networking paradigms, such as SDN or ICN, be a better fit for
quantum networks?

It would be best if quantum networks could seamlessly integrate into existing
networks in order to avoid having to reinvent the wheel, to benefit from
developments happening in the classical networking community, and to reduce
deployment complexity and costs. Perhaps this itself is worth stating as a goal
or principle.

It would be worthwhile to add some text (perhaps an entire section) on this
topic with illustrative diagrams.

#### Deployment issues

The technology is at an early stage. It would be interesting to add a section
on how the technology could be deployed over time in a way where upgrades are
easy to roll out as the technology matures.

#### How to deliver the fidelity part of the service

Would it be worth mentioning some general principles necessary for protocols
that try to ensure the fidelity of the delivered service? How do we specify
classes of service.

One possibility for this draft would be to simply explain the mechanisms by
which fidelity is lost and the necessary parameters and equations that govern
its behaviour. Additionally, the gain and costs of entanglement distillation
would have to be explained in more detail.

Because of the importance of fidelity, it might be worth adding an entire
section/sub-section of its own.

#### Resources in a quantum network

What are the resources that we need to consider in a quantum network. Memory?
Entangled pair generation rate? Fidelity? Time?

This section can tie in with the RuleSets draft which explicitly collects
information about the available resources within the network.

#### Routing, scheduling, and traffic engineering

Do we need to say anything about the location of the Bell Pairs? If we have a
quantum network with some large number of quantum nodes, we need to make a
decission about to which pairs of nodes we need to distribute Bell Pairs. This
depends on where the applications are running.

This leads into a discussion of "routing". If the application requires a Bell
Pair to be present at nodes A and Z, we need a quantum routing function to
determine the optimal path A, B, C, ..., Z of nodes where to perform swap
operations to create the A and Z Bell pair.

It also leads to a discussion of "scheduling". We need to decide when to create
the A and Z Bell pairs. One extreme is to do it "on demand" when the
application needs them. But that is too slow. Thus, the network control
function needs to decide in advance when and where to create Bell pairs in
anticipation of future application demand.

Since "routing" or "scheduling" are going to major functions of any network
protocol to be designed, it might be worthwhile to mention these functions
somewhere in this draft.

It is beyond the scope of the document to design and develop these mechanisms,
but to what degree should the issue of routing, scheduling, and traffic
engineering questions be addressed in this draft?

### Minor issues

These issues are worth discussing, but don't entail significant changes to the
content.

#### Title of the draft

The original title "Architectural Principles of a Quantum Internet" is quite
strong and may discourage participation from non-quantum experts (and it is an
important goal of this draft to encourage these experts to participate). Some
other suggestions include:

* Architectural Principles of a Quantum Internet - an Introduction
* A First Look at the Architectural Principles of a Quantum Internet

#### Bell Pair generation rate is part of the service

Is it worth making this explicit as a principle or should this be part of the
"Time is an expensive resource" point?
