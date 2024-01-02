## [M-#] Looping through the players array (having variable length) to find duplicates in `PuppyRaffle::enterRaffle`(..), can lead to a potential DOS attack as it will increase gas cost for future entrants.

**Description:** <br />
The `PuppyRaffle::enterRaffle` function loops through the players array to check for duplicates. As the length of the 'players' array is not fixed and increases, the gas costs and the number of checks a new player must carryout also increase. This issue has the potential to deter players that enter later due to the remarkably higher gas costs.

```javascript
// Pseudocode for the root cause
function loopThroughPlayersArray(playersArray) {
  for (let i = 0; i < playersArray.length; i++) {
    /*Check for duplicates*/
  }
}
```
**Impact:** <br />
This will cause a rush at the beginning of the raffle for entrants to enter. An attacker might the `PuppyRaffle::entrants` array so big, forcing no one else to enter, ensuring themselves the win! 

**Proof of Concept** <br />
If we have 2 sets of 100 players each entering the raffle:
1st set of 100 players: ~70,000 gas
2nd set of 100 players: ~210,000 gas
Note: The second set of players face a gas cost more than 3 times that of the initial set.

**Recommendated Mitigation** <br />
- Avoid checking for duplicate players as a user can anyway enter the raffle using multiple wallets.
- Check for duplicates using a mapping data structure rather than iterating over an array
- Consider using Openzepellin's Enumerable set contracts which is used for storing unique values only.