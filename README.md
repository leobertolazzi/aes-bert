# AES with BERT based models

This repository contain part of the material used for the exam of the course "Machine Learning for NLP I" (M.Sc. Cognitive Science, UniTn/CIMeC).

Automated Essay Scoring (AES) is the task of scoring written text employing automated computational methods. The first attempts in developing AES systems trace back to more than 50 years ago and the fact that almost all linguistic dimensions (semantics, grammar, style, persuasiveness, etc.) play a role in this task makes it very interesting. 

SOTA approaches on AES are all supervised and the task can be framed in three different ways: regression, classification, or ranking. The possible algorithms ranges from linear and logistic regression to deep neural networks.
The most widely adopted evaluation metric is Quadratic weighted Kappa, other widely used metrics include error metrics such as Mean Absolute Error and Mean Square Error and correlation metrics such as Pearson’s Correlation Coefficient and Spearman’s Correlation Coefficient.

Here I want to test the different performance of the original [BERT](https://arxiv.org/abs/1810.04805) and other BERT based models, namely [RoBERTa](https://arxiv.org/abs/1907.11692), [DistilBERT](https://arxiv.org/abs/1910.01108), and a [DistilRoBERTa](https://huggingface.co/sentence-transformers/all-distilroberta-v1) from the [Sentence-Transformers](https://www.sbert.net/index.html) library, when used as feature extractor to perform AES.

## Dataset

The dataset I am using to perform AES is the ASAP dataset, which was released in 2012 as part of a Kaggle competition. More precisely, I am using only the part of the dataset that was originally released as training set. The dataset can be found in the folder `data`.

More information about ASAP are available at the [original Kaggle page](https://www.kaggle.com/competitions/asap-aes/overview).

Information about current SOTA on the dataset are available on [Papers with Code](https://paperswithcode.com/dataset/asap).

## Approach

AES is performed here as a regression task and following these steps: 

1. All scores are standardized w.r.t. their relative essay set

2. The first 512 tokens of each essay are encoded either by BERT or the other three BERT based models and the average of the tokens embedding is taken as essay embedding

2. A MLP is trained to predict standardized scores from essay embeddings

3. All scores are re-scaled to their original range

The training is done using 5-fold cross-validation for each of the four pretrained models.

The chosen evaluation metric is Quadratic weighted Kappa (QWK). 
QWK is a measure of interrater agreement between two raters that provide discrete numeric ratings. Potential values range from -1, representing complete disagreement, to 1, representing complete agreement. A kappa value of 0 is expected if all agreement is due to chance.

## Results

| Approach | meanQWK | maxQWK | minQWK |
|----------|---------|--------|--------|
| BERT | 0.7203 | 0.7388 | 0.6979 |
| DistilBERT | 0.7091 | 0.7185 | 0.6990 |
| RoBERTa | 0.7288 | 0.7562 | 0.7068 |
| sentBERT | 0.6825 | 0.6930 | 0.6689 |
| Human | 0.7537 | 0.7537 | 0.7537 |
| SOTA | 0.7910 | 0.7910 | 0.7910 |

The best performing model is RoBERTa (0.7288 after 5-fold cross-validation), which achieved a max agreement almost identical to human level (0.7562 vs. 0.7537). The second best model is BERT (0.7203) and the third one is DistilBERT (0.7091). The DistilRoBERTa from Sentence-Transformers obtained the lowest agreement overall (0.6825). 

RoBERTa obtained better results than the other models probably because of its training: in fact, while maintaining the same size of the original BERT model, it is trained using 10x training data.

The results obtained by DistilRoBERTa show that the training approach used for models in the Sentence-Transformers library might not be beneficial for tasks like AES, while it was for standard Semantic Text Similarity tasks.

## Credits
The visualizations I use are adapted from [https://github.com/Turanga1/Automated-Essay-Scoring](https://github.com/Turanga1/Automated-Essay-Scoring)
