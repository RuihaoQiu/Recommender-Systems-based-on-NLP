## Job Title Recommender
This system attempts to help the users to realize that there are more options about their position title.

### Job title autocomplete
The first function should also be the autocomplete as I described in the previous chapter. We had make a lot of efforts to build a standard job title catalogue that provide us about 10k standard job titles from our global labor market database. The difference from the location autocomplete is the ranking algorithm part. The ranking score can be simply the popularity of job title, the logic behind it is also simple, recommend the most popular job title depends on input of the user.

### Recommend title by similarity
One user might not have only one job title, for example my job title can be data scientist, machine learning engineer, artificial intelligence expert, or something I did not realize. Then this recommender can help me to find out what title I should have.

Let see some predictions first[1, 2]:

```
def find_similar_titles(input_title, top_n=5):
    """predict the most similar titles."""
    id_ex = inv_title_dict[input_title]
    skill_vector = skill_dict.doc2bow(skillss[id_ex])
    tfidf_vector = tfidf[skill_vector]
    lsi_vector = lsi_model[tfidf_vector]
    sims = sorted(enumerate(index[lsi_vector]), key=lambda item: -item[1])
    out_list = [(titles[i], s) for i,s in sims[1:top_n+1]]
    return out_list
```
```
job_title = "machine learning engineer"
find_similar_titles(job_title)
```
[('ai engineer', 0.93368685),<br>
 ('ml engineer', 0.9050194),<br>
 ('machine learning scientist', 0.9023429),<br>
 ('artificial intelligence engineer', 0.9012699),<br>
 ('learning software engineer', 0.8630173)]


```
job_title = "python developer"
find_similar_titles(job_title)
```
[('python django developer', 0.8588372),<br>
 ('python engineer', 0.85697204),<br>
 ('backend engineer', 0.8015604),<br>
 ('go developer', 0.774391),<br>
 ('api engineer', 0.76543325)]


The most cruicial part is not modeling or prediction, instead is the data prepocessing. Thanks to our standard job and skill catalogues, we had developed two important tools - job title standardizer and skill extractor, with them we are able to get the most important skills for each standard title. I will go through more details in next chapter.

After prepocessing, things become easy, as we can see from above, we can vectorize each job title by tfidf embedding[3] followed by LSI[4] demension reduction, then construct the similarity matrix from the vectors.

#### Reference
1. [Notebook with complete code](https://github.com/RuihaoQiu/Recommender-Systems-based-on-NLP)
1. [gensim tutorial](https://radimrehurek.com/gensim/auto_examples/core/run_similarity_queries.html#sphx-glr-auto-examples-core-run-similarity-queries-py)
1. [TFIDF wiki](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)
1. [LSI (Latent Semantic Analysis) wiki](https://en.wikipedia.org/wiki/Latent_semantic_analysis#Latent_semantic_indexing)
