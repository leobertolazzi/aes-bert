# AES with BERT embeddings

This notebook is part of the material used for the exam of the course "Machine Learning for NLP I" (M.Sc. Cognitive Science, UniTn/CIMeC).

Automated Essay Scoring (AES) is the task of scoring written text employing automated computational methods. The first attempts in developing AES systems trace back to more than 50 years ago and the fact that almost all linguistic dimensions (semantics, grammar, style, persuasiveness, etc.) play a role in this task makes it very interesting. 

SOTA approaches on AES are all supervised and the task can be framed in three different ways: regression, classification, or ranking. The possible algorithms ranges from linear and logistic regression to deep neural networks.
The most widely adopted evaluation metric is Quadratic weighted Kappa, other widely used metrics include error metrics such as Mean Absolute Error and Mean Square Error and correlation metrics such as Pearson’s Correlation Coefficient and Spearman’s Correlation Coefficient.

## Dataset

The dataset I am using to perform AES is the ASAP dataset, which was released in 2012 as part of a Kaggle competition. More precisely, I am using only the part of the dataset that was originally released as training set. The dataset can be found in the folder `data` .

More information about ASAP are available at the [original Kaggle page](https://www.kaggle.com/competitions/asap-aes/overview).

Information about current SOTA on the dataset are available on [Papers with Code](https://paperswithcode.com/dataset/asap).

## Approach

AES is performed here as a regression task and following these steps: 

1. All scores are standardized w.r.t. their relative essay set

2. The first 512 tokens of each essay are encoded by BERT and the average of the tokens embedding is taken as essay embedding

2. A MLP is trained to predict standardized scores from essay embeddings

3. All scores are re-scaled to their original range

The training is done using 5-fold cross-validation.

The chosen evaluation metric is Quadratic weighted Kappa (QWK). 
QWK is a measure of interrater agreement between two raters that provide discrete numeric ratings. Potential values range from -1, representing complete disagreement, to 1, representing complete agreement. A kappa value of 0 is expected if all agreement is due to chance.

## Credits
The visualizations I use are adapted from [https://github.com/Turanga1/Automated-Essay-Scoring](https://github.com/Turanga1/Automated-Essay-Scoring)