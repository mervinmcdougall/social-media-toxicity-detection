

# Detecting and Mitigating Toxic Comments in Online Platforms Using Toxic Bert and Topic Modeling 

**Group #1**

Mervin McDougall, Marina Mitiaeva, Lakshmi Peram, Nick DeVita

## 

## Objective

This project aims to enhance Reddit's moderation process and foster a healthier environment by introducing a **proactive system** **to detect high-risk topics that are likely to attract viral toxic comments**. Our study will analyze a dataset of questions and top-voted answers to (1) identify highly engaging topics using metrics like upvote scores and (2) assess the toxicity of comments within those threads using the Toxic-BERT model, evaluating the relationship between toxicity and engagement. In the future, this model could be deployed for **real-time analysis**, enabling moderators to efficiently respond to harmful content and promote safer community interactions.

The problem we address is **business-oriented**, focusing on improving the quality and sustainability of Reddit’s engagement. Current moderation efforts on Reddit rely on a multi-layered approach that includes **automated tools, human review, and user reports**. While Reddit claims to be "constantly building and refining tools to proactively identify content and behavior that violates policies," [^1]  its **use of NLP remains limited, with notable efforts in harassment detection using** **large language models (LLMs)** only recently emerging before the company’s IPO​ [^2]. Despite these advancements, two-thirds of the moderation process remains manual, placing significant pressure on volunteer moderators. Additionally, Reddit’s reliance on unpaid moderators has made it vulnerable to disruptions, as evidenced by the 2023 protests when moderators opposed new data access fees for third-party app developers​ [^3].

Our approach introduces a **novel, preventive solution** by combining **topic modeling with toxicity detection**, enabling early detection of viral discussions with toxic elements. Unlike reactive measures such as user reports or keyword-based filtering, our system will **proactively identify high-risk conversations** with both high engagement and toxicity. By integrating this solution with Reddit’s current moderation tools, we aim to **reduce manual effort, enhance efficiency, and improve moderation workflows**.

The key stakeholders are **Reddit users**, whose well-being depends on positive interactions; **moderators**, who require better tools to manage content efficiently; **shareholders**, given the company’s recent IPO, with vested interest in platform sustainability; **advertisers**, whose brand safety relies on a healthy, well-moderated platform environment.

## Data Set Description

For our study, we will use the Reddit Questions with Best Answers dataset [^4], released by Nils Reimers, VP of AI Search at Cohere, [^5] and hosted on Hugging Face, a platform renowned for high-quality datasets and machine learning models. The dataset contains **1,833,556 rows** across **4 columns**, representing individual Reddit discussions. Each discussion includes the following features: **1\) title** \- the question text; **2\)** **body** \- detailed context or background of the question; **3\)** **answers** \-  top-rated responses in JSON format; **4\) score** \- the upvote count.

The dataset description specifies that only entries meeting the following criteria were included to ensure quality and relevance: upvote score of at least 3; title length of at least 20 characters; body length of at least 100 characters. These criteria ensure that the dataset provides meaningful content suitable for in-depth analysis. Although some metadata is missing, the dataset remains reliable due to Hugging Face’s standards for documentation, reproducibility, and quality control. At this stage, we do not plan to use additional datasets. However, if further resources become necessary, we will prioritize Hugging Face datasets due to their quality, reliability, and ease of integration into our study.

## Methods

In this project, we will leverage **unsupervised and supervised learning techniques** to uncover hidden topics in Reddit posts, assess their popularity, and evaluate the toxicity of associated answers. The process will follow the following steps:

1. **Data transformation and text preprocessing:** to prepare the data for analysis, we will parse and flatten the answers column, which is currently in JSON format. Numerical features, such as the upvote score, will be scaled to allow for meaningful comparisons across posts. For text columns, we will apply **t**okenization, lowercasing, and remove non-alphabetic characters and URLs. Given the differing needs of topic modeling and toxicity detection, we will apply distinct preprocessing methods. Topic modeling generally benefits from more extensive preprocessing, such as removing stopwords, to improve topic coherence and clarity. In contrast, toxicity detection will rely on transformer tokenizers, which capture word dependencies. Stopword removal would be counterproductive in this context, as it could disrupt the contextual understanding necessary for accurate classification.  
2. **Topic Modeling:** we will use Latent Dirichlet Allocation (LDA), an unsupervised learning algorithm that identifies hidden themes by grouping words based on co-occurrence patterns. Each question will be represented as a mixture of topics, and we will evaluate the coherence scores of the generated topics to ensure they are meaningful and interpretable. Furthermore, the identified topics will be analyzed against upvote scores to explore which themes resonate most with the Reddit community. This analysis will provide insights into topic popularity, helping us understand which discussions are more likely to attract high engagement.  
3. **Toxicity Detection:** to assess how the identified topics correlate with toxic behavior, we will apply the Toxic-BERT \[5\] model from Hugging Face supported by Spark NLP. Toxic-BERT is a fine-tuned version of BERT, designed to detect various forms of toxicity, including general toxicity, severe toxicity, obscene language, threats, insults, and identity-based hate speech. This model will be applied to the top answers associated with each question, classifying responses into relevant toxicity categories. Toxic-BERT is a well-established tool, with 369,532 downloads in the past month, highlighting its popularity and effectiveness within the NLP community. This analysis will help identify which topics are more likely to attract toxic behavior and require closer monitoring.  
4. Evaluation and Model Integration: topic modeling results will be assessed using coherence scores to ensure the topics are relevant and interpretable. For toxicity detection, we will measure performance using precision, recall, F1-score, and accuracy, with a primary focus on the F1-score to account for any potential class imbalances. The final model will integrate the outputs from LDA and Toxic-BERT to flag posts containing topics prone to toxic discussions. These flagged posts will be further evaluated to ensure the predictions align with real-world moderation needs, supporting moderators by prioritizing posts for manual review. This approach will enhance both the efficiency and effectiveness of Reddit's moderation efforts, contributing to a safer and healthier online environment.

## Risks, concerns, and limitations

Several risks could impact our project, including:

1) **Data Quality and Bias:** The dataset may contain biases, noise, or inconsistencies that could affect the performance of both topic modeling and toxicity detection. To mitigate this, we will perform thorough data exploration and preprocessing, such as identifying outliers, handling missing values, and ensuring a balanced dataset.  
2) **Analysis and Modeling Challenges:** There is a risk that LDA may not yield sufficiently meaningful or interpretable topics. In case of poor performance, we will experiment with alternative topic models (e.g., Non-Negative Matrix Factorization). Another risk is that the model could generate false positives or fail to capture nuanced toxic behavior, such as sarcasm, implicit hate speech, or cultural nuances. In case of poor performance, we will experiment with alternative NLP models.  
3) **Language Variants and Cultural Norms:** we do not know if the language in the data set is the US or UK version. Thus, words acceptable in one country may carry offensive connotations elsewhere (e.g., "fanny pack" in the U.K.). The model’s effectiveness may differ when applied to British English or other English dialects. Politeness norms and sarcasm can further complicate classification, as these factors vary widely across cultures. To address this, we can employ a polyglot model to detect language variants and filter for U.S. English content, ensuring model consistency.  
4) **Resource Constraints and Processing Efficiency:** Handling large-scale datasets like Reddit discussions can be computationally expensive and time-consuming. If resource limitations arise, we will optimize the pipelin**e** (e.g., batching processes, using distributed computing, or exploring model distillation).
 

## References

[^1]:Reddit. (n.d.). *Content Moderation, Enforcement, and Appeals*. Retrieved from [Reddit Help](https://support.reddithelp.com/hc/en-us/articles/23511059871252-Content-Moderation-Enforcement-and-Appeals#:~:text=When%20our%20automated%20systems%20find,Policy%2C%20it%20is%20removed%20automatically).  
[^2]:O’Donnell, L. (2024, March 7). *Reddit introduces AI-powered harassment filter*. Retrieved from [The Register](https://www.theregister.com/2024/03/07/reddit_introduces_aipowered_harassment_filter/).  
[^3]:CNA. (2024). *Reddit may need to ramp up spending on content moderation, analysts say*. Retrieved from [ChannelNews Asia](https://www.channelnewsasia.com/business/reddit-may-need-ramp-spending-content-moderation-analysts-say-4214801).  
[^4]:Hugging Face. (n.d.). *Reddit Questions with Best Answers Dataset*. Retrieved from Hugging Face Datasets.  
[^5]:Hugging Face. (n.d.). *unitary/toxic-bert*. Hugging Face. Retrieved from [https://huggingface.co/unitary/toxic-bert](https://huggingface.co/unitary/toxic-bert)

