# Movie Recommendation System

## Project Overview

This project focuses on developing a movie recommendation system. In an era of exponentially growing digital content, recommendation systems play a vital role, especially in the film industry. They assist users in discovering films aligned with their unique preferences from a vast array of choices, thereby enhancing user engagement and platform loyalty. This system aims to provide relevant and personalized movie suggestions, enriching the user's viewing experience and encouraging broader content exploration.

**Key Objectives:**
1.  **Address Information Overload:** Help users efficiently navigate and select from thousands of available movie titles.
2.  **Enhance User Experience:** Improve user satisfaction by providing accurate and personalized recommendations.
3.  **Promote Content Discovery:** Introduce users to new films or genres they might not have discovered otherwise.

## Dataset

The project utilizes the `movies_dataset.csv` dataset. Key features from this dataset used for building the recommendation models include:
* `title`: The title of the movie.
* `genre`: The genre(s) of the movie.
* `description`: A brief synopsis or description of the movie.
* Other relevant attributes (e.g., `year`, `country`, `avg_vote`, etc., as explored in the notebook for the attribute-based model).

## Methodologies

Two primary approaches were explored to build the recommendation system, particularly addressing the absence of explicit user rating data:

1.  **Model 1: Content-Based Filtering (TF-IDF)**
    * This model recommends movies similar to a user's preferred movie based on its content attributes.
    * It primarily leverages textual features like movie `genre` and `description`.
    * **TF-IDF (Term Frequency-Inverse Document Frequency)** is used to vectorize these textual features, converting them into a numerical format.
    * **Cosine Similarity** is then calculated between the TF-IDF vectors of movies to determine their similarity. Movies with higher cosine similarity scores to a given input movie are recommended.

2.  **Model 2: Attribute-Based Item Filtering**
    * This model also focuses on item similarity but expands beyond textual features to include other categorical and numerical attributes of the movies.
    * Due to the lack of explicit user interaction data (e.g., ratings), this model serves as an item-to-item recommendation approach based on shared or similar attributes.
    * Features such as `year`, `country`, `avg_vote`, `genre` (potentially one-hot encoded or otherwise processed), and `description` (possibly through TF-IDF or other embeddings) are combined to create a comprehensive feature vector for each movie.
    * **Cosine Similarity** is used to find movies that are most similar based on this combined attribute vector.

## Project Workflow

1.  **Data Loading & Initial Exploration:** The `movies_dataset.csv` is loaded, and an initial analysis is performed to understand its structure and content.
2.  **Data Preprocessing:**
    * Handling missing values.
    * Text cleaning for features like `description` and `genre`.
    * Feature extraction (e.g., TF-IDF vectorization for Model 1).
    * Feature engineering and scaling for numerical/categorical attributes for Model 2.
3.  **Model Development:**
    * **Content-Based Filtering:** Implemented using TF-IDF on concatenated `genre` and `description` (or similar textual combinations as per the notebook), followed by cosine similarity.
    * **Attribute-Based Item Filtering:** Implemented by creating a consolidated feature vector for each movie from its various attributes and then applying cosine similarity.
4.  **Evaluation:**
    * Since explicit user feedback is unavailable, a proxy for relevance was used: **genre similarity**. A recommended movie is considered relevant if it shares at least one genre with the input movie.
    * Metrics used: **Precision@10** and **Recall@10**.
        * `Precision@k`: The proportion of recommended items in the top-k set that are relevant.
        * `Recall@k`: The proportion of relevant items found in the top-k recommendations.

## Results

The performance of the two models was evaluated using Precision@10 and Recall@10, based on the genre similarity proxy:

* **Model 1 (Content-Based Filtering):**
    * Average Precision@10: **0.52**
    * Average Recall@10: **0.004**
    * This indicates that, on average, about 52% of the top 10 recommendations were relevant (shared at least one genre). The recall is low, suggesting that while the top recommendations are somewhat relevant, the model covers a small fraction of all potentially relevant items in the dataset with k=10.

* **Model 2 (Attribute-Based Item Filtering):**
    * Average Precision@10: **1.00**
    * Average Recall@10: **0.01**
    * This model demonstrated excellent precision, meaning all top 10 recommendations were highly relevant according to the genre proxy. The recall, while higher than Model 1, remains low.

The low recall values for both models, especially with k=10, can be attributed to the broad definition of relevance (sharing just one genre) and the large number of potentially relevant movies in the dataset compared to the limited number of recommendations presented. However, the strong precision, particularly for Model 2, highlights the models' effectiveness in delivering high-quality recommendations at the top positions.

## Conclusion

This project successfully demonstrates that both Content-Based Filtering and an Attribute-Based Item Filtering approach can be developed to provide useful movie recommendations, even with limitations such as the absence of explicit user interaction data.

Model 2, leveraging a broader set of movie attributes, showed superior precision. The choice between these models, or the development of a hybrid approach, can be tailored to specific platform needs and user preferences. This project lays a solid foundation for further exploration in building more sophisticated, personalized recommendation systems with more comprehensive evaluation methods.

## Technologies Used

* **Programming Language:** Python
* **Key Libraries:**
    * Pandas (for data manipulation and analysis)
    * Scikit-learn (for TF-IDF, cosine similarity, Min-Max scaling, and other machine learning utilities)
    * Jupyter Notebook (for development and presentation)
