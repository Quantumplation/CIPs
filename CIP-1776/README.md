---
CIP: 1776
Title: The Non-Tyrannical Alternative; A Refutation of IOG's Proposed Decentralized Governance Model
Category: ??
Authors:
    - Thomas Stokes <info@eutxo.pro>
Implementors: N/A
Discussions:
    - https://github.com/cardano-foundation/CIPs/pull/?
Status: ??
Created: 2023-01-22
License: CC-BY-4.0
---

# The Non-Tyrannical Alternative: A Refutation of IOG's Proposed Decentralized Governance Model
"It is dangerous to be right in matters on which the established authorities are wrong" 
-Voltaire

## Abstract
We refute the deeply flawed [CIP-1694](https://github.com/JaredCorduan/CIPs/edit/voltaire-v1/CIP-1694/README.md) proposed by Input Output Global for an on-chain governance system and propose an alternative, more decentralized system of governance incorperating and improving upon Project Catalyst.

Currently funds are dispersed from the Cardano Treasuary under an approval/disapproval voting system that exposes our system of governance to a number of critical vulnerabilities and perversely incentivized attack vectors that are routinely exploited by hostile actors. Here we propose a new method, **non-incentivized iterative block approval voting**, which is a variant of multiwinner approval voting modified to resolve the Burr dilemma.

Under our proposal **any Cardano user** will be able to submit a governance action. The release of funds from the Cardano treasuary, including funds previously dispersed by Project Catalyst, will be considered a governance action. The ratification of governance actions will be the responsibility of the following parties:

1. -------redacted----------
2. Stake pool operators & delegation representatives.
3. All Cardano users.

This CIP calls for the merger of SPO and DRep roles for reasons that will become clear later.

## Motivation
The proposal tabled by IOG is *deeply* flawed. The proposal effectively creates a slow-moving pseudo-decentralized system of soft-tyrannical top-down governance. 

Their proposed system of governance makes it incredibly difficult for the community to replace the Constitutional Committee if it is not performing well or is not acting in the best interest of the network. This is because the process for replacing the committee is arbritrarily complex. Additionally, the size and quorum requirements of the committee can change each time it is replaced, which makes it difficult for the community to predict or understand how the committee operates. It also can be difficult to understand how the action will be ratified, and which actions will be enacted or dropped.

Another systemic flaw is that the ratification requirements for different governance actions are arbritrarily complex. Different actions require different combinations of approval from the Constitutional Committee, DReps, and SPOs, and have different thresholds for the percentage of active voting stake and stake held by stake pools that must be met in order for the action to be ratified. Additionally, the fallback conditions are not clear. It could be challenging for community members to keep track of these requirements and understand how to participate in the governance process effectively - dramatically reducing governance participation. 

The core issues summarized:

1. Arbritrary complexity: The proposal introduces several new concepts and mechanisms, such as the Constitutional Committee, DReps, and new types of voting certificates, which makes the system unneccisarily complex for stakeholders - this will reduce overall participation in governance.

2. Poor intentive structure: The proposal mentions that DReps need to be compensated for their work, but it is not clear how this will be done or what the exact mechanisms will be. It fails to leverage the existing incentive structures of the Cardano blockchain. 

3. Bootstrapping problem: The proposal relies on the presence of sufficient DReps to transition the system from its current state to decentralized governance. It is not clear how this will be achieved in practice.

4. Perverse incentives: The proposal as put forward by IOG is designed to entretch existing powers rather than truly grant control over the Cardano Blockchain to the community. 



