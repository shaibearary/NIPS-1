# Rockets
A Rocket is a self-organising unorganization that exists to solve a problem. 

Rocket state exists as multiple event chains which can be compiled to produce the current state of the Rocket.

Consensus over Rocket state is based on **Votepower** which is defined in the [Nostrocket Unprotocol](#).

🚀‼️🍌🔹`ignitionTag` 
## Creating a new Rocket
A Rocket begins with a Rocket Ignition Event which serves as the anchor point for all activity related to the Rocket.

### Rocket Ignition Event 🔂
* `.Kind` 15171031
* `.Tags`
	* [MUST]: `ignitionTag`
	* [MUST]: `t` with the label `name`. This is the Name of the Rocket (`MUST be unique`; `length < 21`; `/^\w+$/`)
	* [SHOULD]: `e` pointer to a `kind 15171971` Problem ANCHOR
	* [OPTIONAL]:`e` pointer to a parent Rocket with the label `parent`

##### Client Validation
`n` MUST be unique && `length < 21` && `/^\w+$/`.  
IF a Problem anchor is tagged, the Problem MUST be created by the same pubkey.   
The pubkey MUST exist in the parent Rocket's Identity Tree if specified.

### Rocket Metadata Event
* `.Kind` 31108
* `.Tags`
	* [MUST]: `ignitionTag`
	* [MUST]: `e` pointer to the `kind 15171031` Rocket Ignition event with the label `rocket`
	* [MAY]: `a` pointer to a `kind 31107` Problem anchor
	* [OPTIONAL]: `e` pointer to a `kind 15171032 ` Identity Tree with the label `Identity`. Taken from parent Rocket if not specified.
	* [OPTIONAL]: `e` pointer to a `kind 15171032 ` Identity Tree with the label `Maintainers`. Taken from parent Rocket if not specified.
	* [OPTIONAL]: `m` the mission of this rocket 
* `.Content` If this Rocket does not point to a Problem, the purpose of the Rocket should be specified in the content. 

## Merits and Votepower


## Consensus over State
Consensus should be used as sparingly as possible and only for the most critical aspects of the Nostrocket Unprotocol:  

* Merit Requests and voting
* Payments (payment for products => compensation and dividends)

HEAD pointer (helper) - replaceable event by anyone with votepower
Consensus events are like tree merges but by Votepower instead of Maintainers

Protocol Flow:  
1. Participant publishes a valid state change event.  
2. Anyone with `Votepower > 0` merges the event into their consensus chain and updates their HEAD to point to the latest consensus event.
3. Anyone else can immediately get the same state by following the same chain. If we have votepower we produce our own consensus chain while doing this.
4. If we see mismatched consensus chains, we follow the one with the most votepower.


### Consensus Event
* `.Kind` 15172008
* `.Tags`
	* [MUST]: `ignitionTag`
	* [SHOULD]: `e` pointer to the `kind 15171031` Rocket Ignition event with the label `rocket`
	* [MUST]: `e` pointer to the state change event being witnessed, with the label `request`
	* [MUST]: `e` pointer to the last valid consensus event published by this pubkey, with the label `previous`
	* [SHOULD]: `h` `<current bitcoin height>:<height of this event in this pubkey's consensus chain>`

### Consensus HEAD Event
This event stops us from producing extra consensus events while catching up to the current state.

- Rocket Anchor
  - Merit Requests
  	- Merit Votes
  - 

# Consensus State (requires consensus events for every state change)
- Merit Table (requests, votes, payments)
- Protocol Rules
- Identity Tree
- Maintainer Tree

Instead of validating identity tree inclusion as a normal client operation, only do it for creating a consensus event. When rebuilding state, don't validate. This solves problem of changes to the tree. If votepower created a consesnsus event we assume they at least were in the tree at that time.

