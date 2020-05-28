## Location Recommender
Start with a simple function that can suggest the full location based on the user's initial inputs.

### Autocomplete
There should be two important parts:
#### Searching algorithm
I have already implemented a autocomplete function using Trie structure in one of my previous projects - [Location autocomplete](https://algonotes.readthedocs.io/en/latest/Trie.html#autocomplete).
The basic idea is to suggest full location based on the users input in real time. For example: <br>

The user input: `ber`<br>
The output suggestion:<br>
    `berlin, berlin, germany`<br>
    `bern, bern, switzerland`<br>
    `bertoua, est, cameroon`<br>
    `bergen, hordaland, norway`<br>
    `bergamo, lombardy, italy`<br>

The order of the result is determined the ranking algorithm.

#### Ranking algorithm
Generate a score for each results. The score depends the features we select based on different problems. For the location problem, the features can be the population, size, economics, importance of the location.

If we have the information about the user location, language and preference of the user, by incorporated them, the user will get the most relevant suggestions based on his/her own preferences. <br>
Took the above example, a user based in Norway, might have `bergen, hordaland, norway` as his first option.

Furthermore, once we have the user's historical searching data, we can further improve the ranking. The system should be able to memorize/learn from the user's history and suggest the more relevant results. From the above example, if the user always searching job in Bergen Norway, then, when he start to type `ber`, `bergen, hordaland, norway` should appear in the first order.
