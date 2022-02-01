# Semester_Project_Code_Change_Rep
The main goal of our code below is to adapt the data of the BugSwarm API to the CC2VEC code so that we can train CC2Vec on the BugSwarm API with the goal of predicting 
if a commit will break the CI of a project. Once adapted, we test the performance of CC2Vec on our dataset and propose some optimizations on it. 
# As github doesn't support large files, please follow the steps towards executing our code: # 
--------
## 1- clone CC2Vec into this repository. link for CC2Vec code : https://github.com/CC2Vec/CC2Vec.git ##
--------
## 2- Add the CC2Vec data+model folder to this repository :  ## 
Please follow the link below in order to download the data and pretrained models of CC2Vec : 
https://drive.google.com/file/d/1rPYGjw87YMNAdb2_2baO967i7ynp8sa2/view?usp=sharing 

# Documentation of the code # 
## CC2Vec_Data_Visualization.ipynb ## 
### get_diff() : ### 
We use the get_diff() function in order to obtain the representation of a commit based on the repository and the commit we pass as inputs to the function, then inside our 
function we split the commit into files and then for each file we have two lists, one for added code and one for removed code. 
### Data creation : ###
We  first call the bugswarm api filter, then we parse the data we have in BugSwarm so that it works with CC2Vec using the get_diff() function. We add other information related 
to the commit so that it can be identical to the CC2Vec data.
### Training the model : ### 
In order to train the CC2Vec model on our dataset, you shoulf first go to the CC2Vec and execute the following command: 
python jit_cc2ftr.py -train -train_data ../bugswarm_beta2.0_training_dataset -test_data ../bugswarm_beta2.0_training_dataset -dictionary_data ../data+model/data/jit/qt_dict.pkl
### Problems encountred : ### 
While training the CC2Vec model on our dataset we had an issue with the CC2Vec code related to array shapes. As our dataset was identical to CC2Vec dataset when visualizing it,
we estimated that the error should be related to a parsing in the command we executed. Thus, we decided o integrate CC2Vec code directly inside our code so that we manage 
to isolate the problem. This notebook is mainly proposed by Florent then we tried to work all of us on it in order to solve the issue related to the adaptation of our dataset to 
CC2Vec. 
## Oussama_Predict_build_result_with_BugSwarm_and_CC2Vec ## 
In this Notebook, we used the unidiff library from python, which is a library that allows us to retrieve directly the representation of commits with files 
instead of doing it manually. Then we call the functions used by CC2Vec in order to train their model on our data.   
