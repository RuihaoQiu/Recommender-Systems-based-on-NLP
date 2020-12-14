## Skill Recommender
In this chapter, we are going to recommend skills for users. This function is to help the users to "discover" their own full skillset. Most of the users know some of their skills, but they might neglect some important/in-demand skills, or they might also want to know what skills to develop in order to keep competitive. In this system, our users just need to input his position, then it will keep recommending the most relevant skills. Our users just need to select from them. This largely improves the user's experience.

### Recommend skills from job title
As I have mentioned in the previous chapter, we have find the most important skills for each standard job title, by using our title standardizer and skill extractor.

The idea behind these two tools are similar, map a series of keywords to a standard title/skill with some customized logics. This requires a standard dictionary of keywords and efficient mapping algorithm. 

The method is as follow:
- Map standard title and extract skill for each job
- Group the skills by title and sort it with tfidf score
- Customized code to filter improper skills and manual QA.

Then we got a title-skills dictionary, which can be used for recommendation. Let's check some results:
```
def find_skills(input_title, top_n=5):
    try:
        return title_skills_dict[input_title][:top_n]
    except KeyError:
        return None
```
```
find_skills("data scientist")
```
['Data Science', 'Python', 'Machine Learning', 'Algorithm', 'Statistics']

```
find_skills("python developer")
```
['Python',
 'Django',
 '(Structured Query Language) SQL',
 'Amazon Web Services (AWS)',
 'Software Development']

```
find_skills("business developer")
```
['Business Development Techniques',
 'English',
 'Prospecting',
 'B2B',
 'Customer Relationship Management (CRM)']

 I would say this simple method make pretty promising results.

### Recommend skills from current skillset
In order to recommend skills from skills, we need the skill correlations. The skill correlation matrix is similar to the title correlation in the previous chapter. However, in skill correlation, I also use skill hierachy to re-order the correlation, therefore skills in the same family tend to be closer to each other.

We don't need the full matrix here, only nearest skills (e.g. top 10) for each skill will be enough. For one skill, we just need to take it nearest neighbors, however, for multiple skills, we take all their neighbors and re-sort them based on their popularity, see the code below: 

```
def sort_skills(input_skill_score):
    """Sort skills by freqency and correlation score"""
    df_temp = pd.DataFrame(input_skill_score, columns=["id", "score"])
    df = df_temp.groupby("id").mean()
    df["freq"] = df_temp["id"].value_counts()
    df_out = df.sort_values(by=["freq", "score"], ascending=[False, False])
    out_id = list(df_out.index.astype(int))
    return out_id

def find_skills(input_skills, top_n=10):
    """Find all correlated skills and sort them"""
    skill_ids = [skill_id_dict[skill] for skill in input_skills]
    out_list = []
    for _id in skill_ids:
        out_list += skill_dict[_id]
    sorted_ids = sort_skills(out_list)
    out_skill_name = [id_skill_dict[x] for x in sorted_ids if x not in skill_ids]
    return out_skill_name[:top_n]
```

Let's see the recommendations:
```
find_skills(["Python", "Java", "Django"])
```
['Programming',
 'C++',
 'HTML5',
 'Java - Hibernate',
 'XML - REST',<br>
 'Perl',
 'Representational state transfe (REST)',
 'XML',
 'Java - Web Application',
 'Ruby ']

Also not bad right?

We can always improve our recommendations by considering more information (e.g. users data) and try different enbeddings.


#### References
1. [Notebook with complete code](https://github.com/RuihaoQiu/Recommender-Systems-based-on-NLP)
​