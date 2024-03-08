# Detecting and classifying MITRE techniques
## Introduction
This project goal is to inspect company reports about cyber-attacks and find the chunks of the documents that are specific to the techniques used by the attackers. So I trained two models (Detector and Classifier) and then used them in a pipeline with an HTML scraper to inspect a document from his url.

## Models training
This project is based on training two models:  
- **Detector:**  given a sentence returns True if the sentence references some cyber-attacks techniques and False otherwise. It was built fine-tuning the pre-trained model **BERT** with our data (a .csv with two columns: *text* and *target*);  
- **Classifier:** given a sentence for which the detector returned True has to return which techniques the sentence is referencing about (from the database of MITRE ATT&CK with 190 techniques). For this model I didn't use a pre-trained model but I trained an LSTM that is one of the best Neural Network to catch relationships in the embeddings of a text.  

## Pipeline 
Once these two models are trained and saved they are available to use in the pipeline. This pipeline has three steps:  
1. Scraping an HTML company report about cyber-security at sentence level. So this chunk of code split an HTML document in sentences and save them in a .csv file.  
2. Using the detector on these sentences to check what sentences reference to some cyber-techniques.  
3. Using the classifier to check the specific techniques for each sentence about cyber-attacks.  

The classifier has to classify the techniques in a sentence from a database of 190 techniques so it's a difficult classification. In order to have more probabilities of success I made the classifier return the 5 best classification output. Now for the evaluation the classifier is right if at least one of the 5 techniques predicted is in the sentence.

## Output  

The output is a .csv file that has on a row:  
- *text:* the sentence of the HTML document in string format;
- *detection:* the boolean result of the detector prediction;
- *detection_confidence:* the confidence of the detector prediction;
- *classification:* a list of the 5 best predictions of the classifier;
-  *classification_confidence:* a list with the confidence of the 5 outputs of the classifier.
