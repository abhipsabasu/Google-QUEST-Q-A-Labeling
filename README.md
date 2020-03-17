# Google-QUEST-QA-Labeling

This code was written for the Kaggle competition **Google QUEST QA Labeling** (https://www.kaggle.com/c/google-quest-challenge/overview). The challenge consisted of predicting a set of **30 features** about the question and answer provided inputs such as **question title, question body, answer body, question user name, answer user name, url etc**. The features that were to be predicted are as follows:
- question_asker_intent_understanding
- question_body_critical
- question_conversational	
- question_expect_short_answer	
- question_fact_seeking	
- question_has_commonly_accepted_answer	
- question_interestingness_others	
- question_interestingness_self	
- question_multi_intent	
- question_not_really_a_question	
- question_opinion_seeking	
- question_type_choice	
- question_type_compare	
- question_type_consequence	
- question_type_definition	
- question_type_entity	
- question_type_instructions	
- question_type_procedure	
- question_type_reason_explanation	
- question_type_spelling	
- question_well_written	
- answer_helpful	
- answer_level_of_information	
- answer_plausible	
- answer_relevance	
- answer_satisfaction	
- answer_type_instructions	
- answer_type_procedure	
- answer_type_reason_explanation	
- answer_well_written

As can be seen from the above list, the columns to be predicted may or may not depend on both question and answer. E.g. **question_type_instructions** may be inferred solely from **question title and question body**, while **answer_relevance** might need to take **question title, question body and answer body** into account. 

### Models
This problem was approached in several steps. First, a **Universal Sentence Encoder** model was used for predicting the outputs. This gave a public LB score of 0.34726 based on **Spearman's Correlation Coefficient** ( and a private LB score of 0.31896).
The problem was then approached by employing **BERT**. The **Huggingface Transformer** implementation was used for **Tensorflow**. Several methods were used in this respect:

- Train BERT models for question and answers separately to predict 21 and 9 features respectively by taking the top layer of BERT only
- Train BERT models for question and asnwers separately but concatenating them for predicting 30 features by using the top layer of BERT 
- Train BERT models for question and answer separately but concatenating them for predicting 30 features and using a LSTM/GRU after concatenating the last 4 layers

Based on the validation results, and also keeping in mind the compute power available in Google Colab, models were saved from the 2nd and 3rd methods. The final result was used after taking an average ensemble of the models created by these two methods. 

### Post Processing
As the evaluation criteria was **Spearman's Correlation Coefficient**, the rank of the data becomes important (https://en.wikipedia.org/wiki/Ranking#Ranking_in_statistics). Looking at the training data, it was found that all values in the training data were only from a small list of numbers. So, a python script was written to round off the predicted values to the nearest number from the above list

### Results

The final ensemble of models gave a **public LB score of 0.43163** and **private LB of 0.39884** (a rank of **72/1571** teams). 
