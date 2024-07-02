# NLP-Project
 
This file contains the most important information about this repository

This repository contains the original challenge, zipped and unzipped:
./clef2024-checkthat-lab-main-task5
./clef2024-checkthat-lab-main-task5.zip

./Documents -> contains the presentation of the project

./Datasets -> contains the training and "test" set cleaned from formatting errors and usless data

./Scripts -> contains all the script used to fix and clean the dataset, run the models and verify the results obtained
./Scripts/FixDataset -> corrects the formatting issues
./Scripts/DatasetCleaning -> removes unwanted data from the tweets
./Scripts/MLChallenge -> code for running Naive Bayes on the challenge
./Scripts/LLMChallenge -> code for running Transformer models on the challenge
./Scripts/verification_score -> calculate result metrics on the classificatio
./Scripts/retrieval_scorer -> calculate result metrics on the evidence retrieval

./Results -> contains all the results obtained
No verson refers to the use of the labelset=label2 (the original challenge labels) for the classificatio
v2 refers to the test on the specifis set of labels chosen for the model (distilBert doesn't have one)
v3 refers to the use of the labelset=label (last set of label tested)