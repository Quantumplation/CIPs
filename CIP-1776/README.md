---
CIP: 1776
Title: The Non-Tyrannical Alternative; An On-Chain Decentralized Governance Mechanism for Voltaire
Authors:
    - Thomas Stokes <info@eutxo.pro>
Contribtors:
    - LOLA
    - AGC
    - PSB
    - ONE
    - Various Parties to be credited later
Implementors: N/A
Discussions:
    - https://discord.gg/nodeshark
Status: ??
Created: 2023-01-22
License: All Rights Reserved
---

# CIP 1776: A Voltaire-Complete On-Chain Decentralized Governance Mechanism
"It is dangerous to be right in matters on which the established authorities are wrong" 
-Voltaire

## Abstract
We refute alternative proposals for Cardano governance and propose a revision of Cardano's on-chain governance system to support the new requirements for Voltaire. Under our proposal any Cardano user will be able to submit a governance action. The ratification of governance actions will be the responsibility of the following parties:

1.	The Septet
2.	Stake Pool Operators (Herein Operators)
3.	Delegated Representatives (Herein Representatives) 

Every governance action (excluding Septet elections) must be ratified by all three of these governance bodies using their votes. Ratified actions may then be enacted on-chain, following a set of well-defined rules.
The Septet shall be composed of seven independently elected representatives responsible for adjudicating the constitutionality of all motions approved by Stake Pool Operators and Delegated Representatives and ratified or overridden accordingly. 
The Stake Pool operators shall act as the upper chamber of governance each casting a single vote while satisfying the eligibility requirements for participation. 

Any Cardano user may register as a Representative and so choose to represent themselves and/or others. They may delegate their voting rights to any other registered Representative. These voting rights will be based on their total ADA holdings, as a whole number of Lovelace. Additionally, Representatives may delegate their collective voting power to other Representatives under an amorphous hierarchical liquid governance model.

Dedications: For my daughter, Gwendolyn, so that the beautiful things of this world do not fall to darkness. And for my son, Theodore, to prove to you there is no great evil in this world that can not be defeated with determination, a true heart, and the courage to stand for what is right.

## Motivation
Growing dissatisfaction with the governance model proposed in CIP1694 has splintered the Cardano community with divisive argumentation and rampant proliferation of misinformation on both sides of the debate. The model proposed by IO, while a commendable attempt to provide a first step to create a technically functional model of governance, fails to bring within its scope a mechanism for the drafting of the Cardano constitution despite its reliance on a constitutional committee. Further, while calling for the Cardano community to “think deeply about the processes for handling the creation of governance actions that are specified in (CIP 1694). In particular, the role of Project Catalyst in creating treasury withdrawal actions” it fails to bring the observed outcome resulting from four years of treasury experimentation into the scope of foundational governance. Finally, the membership of the constitutional committee is somewhat flippantly dismissed as “an off-chain issue”, despite such a committee being (under the CIP1694 model) pivotal to the proper functioning of the governance model, with the risk of a perpetual dissatisfaction in said appointed committee(s) grinding on chain governance to a halt. 

## Goal
The goal of this CIP is to lay down a functional foundation for decentralized decision-making. This CIP describes a model for on-chain governance that is both technically and legislatively complete and (unlike other proposed models of governance) without any missing dependencies for a functional mechanism of minimum viable governance. This model leverages the existing Cardano governance mechanism scheme, correctly utilizing the seven existing governance keys. It aims to provide a tangible, scalable foundation that is technically achievable within a reasonable timeframe for any competent world leading blockchain development organization.

## Current Governance Mechanism Design

The seven-key on-chain Cardano governance system, established during the Shelley ledger phase, possesses the ability to:

1.	Alter protocol parameter values, including the initiation of "hard forks."
2.	Move ADA between reserves and the treasury, as well as the withdrawal of ADA from these sources.

Under the existing framework, initiating governance actions necessitates special transactions with Quorum-Many authorizations from the governance keys (5 of the 7 on the Cardano mainnet). The transaction body contains fields detailing the proposed governance action, such as amending protocol parameters or initiating fund transfers. Each transaction can prompt only one type of governance action, but an individual action can produce multiple effects (e.g., modifying two or more protocol parameters).

Transaction body field no. 6 is utilized for protocol parameter updates. Meanwhile, Move Instantaneous Rewards (MIR) certificates are employed for handling treasury and reserve movements. Authorized governance actions are executed at epoch boundaries (when they are enacted).

Changing the major protocol version enables Cardano to enact controlled hard forks. This type of protocol parameter update therefore has a special status, since stake pools must upgrade their node versions to support the new protocol version once the hard fork is enacted.

## Limitations of the Shelley Governance Model

As we progress towards the Voltaire era, this proposal aims to remedy several limitations of the current design through the application of a minimum viable governance approach. Offering a simple, scalable, mutable model for governance is impossible under the Shelly model and thus an alternative is required.  
The Shelley governance model does not provide an avenue for active on-chain engagement from Ada holders. Although protocol changes often result from discussions with selected community members, the process is primarily driven by the founding entities as IOG, the Cardano Foundation, and Emergo hold between them all seven governance keys. 
Treasury movements are a crucial and delicate matter, but they can be difficult to monitor. It is essential to establish greater transparency and additional layers of control over these transactions.
Hard forks, which require special handling by SPOs, are not distinguished from other protocol parameter alterations.
Lastly, while Cardano's founding entities and many community members share a somewhat unified vision for the project, there is no explicit document outlining these guiding principles. Utilizing the Cardano blockchain to permanently record the shared Cardano ethos as a formal Cardano Constitution makes sense.

## Out of scope

Legal issues
This CIP does not acknowledge the jurisdiction of any nation state or legal authority for the enforcement of any action in relation to the Cardano blockchain due its nature as a censorship resistant distributed network.

## Copyright
© EveryEpoch Limited - All Rights Reserved

