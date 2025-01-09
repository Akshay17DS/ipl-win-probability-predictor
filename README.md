# Movie Recommender System

## Project Overview

The Movie Recommender System simplifies the process of choosing movies by providing personalized recommendations based on movie features. Using a Content-Based Filtering approach, the system analyzes attributes like genres, cast, and crew to compute similarities and suggest movies tailored to user preferences. This project aims to deliver accurate, relevant, and scalable recommendations for users navigating large movie datasets.


## Types of Recommender Systems
1. **Content-Based Recommender System**:  
   Suggests items based on the similarity between item features and the user's preferences or past interactions.
   
2. **Collaborative Filtering Recommender System**:  
   Predicts user preferences by analyzing patterns in the behavior of similar users or items.
   
3. **Hybrid Recommender System**:  
   Combines multiple recommendation techniques (e.g., content-based and collaborative filtering) to improve accuracy and overcome individual limitations.

---

## Project Flow
1. **Dataset Details**  
2. **Pre-processing**  
3. **Model Building**  

---

## Dataset Details
### 1. Movies Dataset (`tmdb_5000_movies.csv`)
Contains information about movies, including:
- **budget**: Production budget (USD)  
- **genres**: Movie genres  
- **id**: Unique movie identifier  
- **original_language**: Movie language  
- **release_date**: Release date  
- **revenue**: Revenue (USD)  
- **runtime**: Movie runtime (minutes)  
- **vote_average**: Average user rating (1-10)  
- **vote_count**: Number of user votes  

### 2. Credits Dataset (`tmdb_5000_credits.csv`)
Provides credits information, including:
- **movie_id**: Unique movie identifier (links to Movies dataset)  
- **cast**: List of actors and their roles  
- **crew**: List of crew members and their roles (e.g., director, writer)  

---

## Pre-processing
1. **Combined both datasets** using the `movie_id`.  
2. **Kept important columns only**, such as:
   - budget  
   - homepage  
   - id  
   - original_language  
   - original_title  
   - popularity  
   - production_company  
   - production_countries  
3. **Created a new column called `Tags`** by concatenating the selected columns.  

---

## Methodology
### 1. Text Vectorization
**Text Vectorization** was performed using the Bag of Words (BoW) model:  
- Tokenized the text to split it into individual words.  
- Removed stop words (like "the", "is") to focus on meaningful content.  
- Created a sparse matrix where each row represents a movie, and each column corresponds to a unique word in the dataset.  

#### Example:
```
Movie 1: "action adventure thriller"  
Movie 2: "adventure fantasy magic"
```

**Vectorized Matrix:**
```
       action  adventure  thriller  fantasy  magic
M1      1         1         1         0       0
M2      0         1         0         1       1
```
![download](https://github.com/user-attachments/assets/6a6e4fc7-f87b-4c60-b515-98e11cbf09fd)
---

### 2. Similarity Calculation
Cosine similarity was used to compute pairwise similarity between movie vectors.  
- Cosine similarity measures the angle between two vectors, making it ideal for text-based comparisons.  

#### Example:
```
Cosine Similarity between Movie 1 and Movie 2:
Similarity = 0.408 (40.8%)
```

---

### 3. Building the Similarity Matrix
A similarity matrix was created where each entry `[i][j]` represents the similarity between movie `i` and movie `j`.

#### Example:
```
[[1.0, 0.408, 0.5],
 [0.408, 1.0, 0.288],
 [0.5, 0.288, 1.0]]
```

---

### 4. Recommendation System
For a given movie, the system:  
- Retrieves its similarity scores from the matrix.  
- Sorts the movies by similarity in descending order.  
- Recommends the top N most similar movies (excluding itself).  

#### Example:
```
Input: "Avatar"
Recommendations:
1. "The Dark Knight Rises" (50% similar)
2. "Pirates of the Caribbean" (40.8% similar)
```

---

## Results
The Movie Recommender System successfully provides personalized movie recommendations.  

#### Example:
```
Input: "The Dark Knight"
Recommendations:
1. Gladiator  
2. The Dark Knight Rises  
3. Sphere  
4. How to Train Your Dragon 2  
5. Jack the Giant Slayer
```


