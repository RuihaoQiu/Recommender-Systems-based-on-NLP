## Location Autocomplete
As first part of this project, let's start with a simple function that can suggest the full location based on the user's initial inputs.
It is also called autocomplete. There should be two important parts: searching and ranking.

### Searching algorithm
I have already implemented an autocomplete function using Trie structure in one of my other project - [Location autocomplete](https://algonotes.readthedocs.io/en/latest/Trie.html#autocomplete), feel free to check it.
The basic idea is to suggest full location based on the users input in real time. For example: <br>

The user input: `ber`<br>
The output suggestion:<br>
    `berlin, berlin, germany`<br>
    `bern, bern, switzerland`<br>
    `bertoua, est, cameroon`<br>
    `bergen, hordaland, norway`<br>
    `bergamo, lombardy, italy`<br>

The order of the result is determined by the ranking algorithm. Our current solution is using google map API, which is good that it provides complete location globally, however, the drawback is it cannot search for region or country automatically, and it is a general API, customized the ranking.

### Ranking algorithm
Generate a score for each results. The score depends the selected features according to different problems. For the location problem, the features can be the population, size, economics, importance of the location etc.

If we have the information about the user location or language, by incorporated them, the user will get the most relevant suggestions based on these implicit information. <br>
Took the above example, a user based in Switzerland, the system might suggest `bern, bern, switzerland` as his first option.

Furthermore, once we have the user's historical searching data, we can further improve the ranking. The system should be able to memorize/learn from the user's history and suggest the more relevant results. From the above example, if the user always searching job in Bergen Norway, then, when he start to type `ber`, the first suggestion appears should be `bergen, hordaland, norway`.
