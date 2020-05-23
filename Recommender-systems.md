## Recommender systems in NLP
This project will introduce several recommender systems in NLP, specifically in the domain of online recruitment. It includes important ideas behind the recommender systems of our web application, also similar with those in recruiting platform like LinkedIn, Xing.

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
Generate a score for each results. The score depends the features we select based on different problem. For the location problem, the features can be the population, size, economics, importance of the location. 

If we have the information about the user location, language and preferance of the user, by incorporated them, the user will get the most relevant suggestions based on his/her own preferances. <br>
Took the above example, a user based in Norway, might have `bergen, hordaland, norway` as his first option.

### Skill recommender

### Profile recommender

### Online training recommender
