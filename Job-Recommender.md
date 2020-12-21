## Job Recommender

Once a user has his full skillset, we can start to recommend jobs. Or the other way round, once a HR has a skillset for one job, we can start to recommend candidates. Let's start with recommending jobs. So our goal is to recommend the most relevant jobs for a user. Mathematically, the relevance can be the silimiarity of two skillsets (user's skills and job's skills). 

### Recommend based on similarity
input skillsets:
```
job_skills = ["Python", "Machine Learning", "AWS", "SQL", "GIT"]

candidates_skills = [
    ["Python", "Machine Learning", "AWS", "SQL", "Deep Learning"],
    ["R", "Statistics", "GIT",  "English"],
    ["Python", "Machine Learning", "AWS", "GIT", "English"]
]
```

#### Jaccard similarity.
The most naive idea, is to count the number of skills appear in both sets. It is also called Jaccard similarity.
```
## modified the Jaccard similarity to matching score

def matching_score(job_skills, candidate_skills):
    if job_skills:
        commom_skills = set(job_skills) & set(candidate_skills)
        score = len(commom_skills) / len(set(job_skills))
        return score
    else:
        return 0
```
```
for i in range(3):
    print("score of candidate %d: %f" % (i, matching_score(job_skills, candidates_skills[i])))
```
score of candidate 0: 0.800000<br>
score of candidate 1: 0.200000<br>
score of candidate 2: 0.800000<br>


#### Cosine similarity
We need make skill embeddings for all the text. Here I use bag of words + tfidf. You are free to further reduce the vector dimension by using Latent semantic indexing(LSI).
```
## bulid dictionary and embedding use gensim
def embedding(skillsets):
    dictionary = corpora.Dictionary(skillsets)
    corpus = [dictionary.doc2bow(text) for text in skillsets]
    model = TfidfModel(corpus)
    index = similarities.MatrixSimilarity(model[corpus])
    return dictionary, model, index

dictionary, model, index = embedding(candidates_skills)
```
```
## compute similarity
def compute_sim(skills):
    job_query = dictionary.doc2bow(skills)
    vec_job = model[job_query]
    sims = index[vec_job]
    return sims

sims = compute_sim(job_skills)
```
```
for i,s in enumerate(sims):
    print("score of candidate %d: %f" % (i, s))
```
score of candidate 0: 0.730248<br>
score of candidate 1: 0.072699<br>
score of candidate 2: 0.531179<br>

#### Word Mover’s Distance (WMD)

WMD allow us the compare two texts by considering the word similarity. In general we will use the pre-computed word vectors. However, in this use case, we have to find a way to compute the skill vectors. Actually it involves two type of NLP tasks, NER (how to identify skills) and skill embedding (how to generate a vector for each skill). Here I assume that we have already had the skill vectors in hand. Then we just need to call the function:
```
model.wmdistance(skillset_1, skillset_2)
```

As mention the this [blog](https://medium.com/@adriensieg/text-similarities-da019229c894), there are many different ways to compute text similarity. The Transformers are becoming popular, it will interesting to do some experiments on them. I am making effort to introduce them into our projects. In the furture, I will start a new section to introduce how the transformers can impact on NLP recommender systems.


### References
- [Gemsim documents](https://radimrehurek.com/gensim/auto_examples/index.html#documentation)
- [TFIDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)
- [Latent semantic indexing](https://nlp.stanford.edu/IR-book/html/htmledition/latent-semantic-indexing-1.html)
- Word Mover’s Distance:
    - [gensim function](https://radimrehurek.com/gensim/auto_examples/tutorials/run_wmd.html)
    - [paper](http://proceedings.mlr.press/v37/kusnerb15.pdf)
- [Text similarity](https://medium.com/@adriensieg/text-similarities-da019229c894)