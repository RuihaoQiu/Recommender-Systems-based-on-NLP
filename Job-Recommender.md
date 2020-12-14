## Job Recommender

Once a user has his full skillset, we can start to recommend jobs. Or the other way round, once a HR has a skillset for one job, we can start to recommend candidates. Let's start with recommending jobs. So our goal is to recommend the most relevant jobs for a user. Mathematically, the relevance can be the silimiarity of two skillsets (user's skills and job's skills). 

### Recommend based on similarity
- Jaccard similarity.
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

- Cosine similarity
