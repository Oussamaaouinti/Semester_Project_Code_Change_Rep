# Semester Project: Code Change Representation #
The main goal of our project is to predict build failure during the Continuous Integration Process. After going through the litterature and the studies related to build failure classification, we have chosen to use code changes representations as input to our prediction model. In order to be able to model code changes, we mainly use CC2Vec (link to CC2Vec: https://arxiv.org/pdf/2003.05620.pdf) which was one of the papers we went through during our literature review). The reason behind our choice is that according to the authors CC2Vec, it outperforms all of the state of the art approaches. Moreover it is language agnostic and its source code is available. 
For our dataset, we used BugSwarm (link to BugSwarm: http://www.bugswarm.org/docs/introduction/what-is-bugswarm/) to retrieve code changes as it has an API that provides fail-pass build pairs. (link to BugSwarm). 
Hereafter the different steps we went through during the project: 
a)	preprocess BugSwarm data to adapt it to BugSwarm Commits. 
b)	train CC2Vec model on BugSwarm Commits.
c)	extract vector representation of Code Change using CC2Vec model. 
d)	Use vectors extracted by CC2Vec as inputs for build classification using supervised ML algorithms

In the notebooks included in this repository, you will find the different steps we went through during the project including the difficulties we faced. 
For further information about the notebooks, read the documentation of code below. 
## Documentation of the code ## 
### CC2Vec_Data_Visualization.ipynb ### 
#### get_diff() : ####
We use the get_diff() function in order to obtain the representation of a commit based on the repository and the commit we pass as inputs to the function, then inside our 
function we split the commit into files and then for each file we have two lists, one for added code and one for removed code. 
#### Data creation : ####
We  first call the bugswarm api filter, then we parse the data we have in BugSwarm so that it works with CC2Vec using the get_diff() function. We add other information related 
to the commit so that it can be identical to the CC2Vec data.
#### Training the model : #### 
In order to train the CC2Vec model on our dataset, you shoulf first go to the CC2Vec and execute the following command: 
python jit_cc2ftr.py -train -train_data ../bugswarm_beta2.0_training_dataset -test_data ../bugswarm_beta2.0_training_dataset -dictionary_data ../data+model/data/jit/qt_dict.pkl
#### Problems encountred : #### 
While training the CC2Vec model on our dataset we had an issue with the CC2Vec code related to array shapes. As our dataset was identical to CC2Vec dataset when visualizing it,
we estimated that the error should be related to a parsing in the command we executed. Thus, we decided o integrate CC2Vec code directly inside our code so that we manage 
to isolate the problem. This notebook is mainly proposed by Florent then we tried to work all of us on it in order to solve the issue related to the adaptation of our dataset to 
CC2Vec. 
### Oussama_Predict_build_result_with_BugSwarm_and_CC2Vec.ipynb ### 
In this Notebook, we used the unidiff library from python, which is a library that allows us to retrieve directly the representation of commits with files 
instead of doing it manually. Then we call the functions used by CC2Vec in order to train their model on our data.   
