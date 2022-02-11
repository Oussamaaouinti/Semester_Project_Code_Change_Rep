# Semester Project: Code Change Representation #
The main goal of this project is to predict build failure during the Continuous Integration Process. After going through the litterature and the studies related to build failure classification, we have chosen to use code changes representations as input to a prediction model. In order to be able to model code changes, we mainly use [CC2Vec](https://arxiv.org/pdf/2003.05620.pdf) which is one of the papers we went through during our literature review. The reason behind our choice is that according to the authors, CC2Vec outperforms other the state of the art approaches. Moreover, it is language agnostic and its source code is available. 

For our dataset, we used the [BugSwarm dataset](http://www.bugswarm.org/docs/introduction/what-is-bugswarm/) to retrieve code changes as it has an API that provides fail-pass build pairs. 
Hereafter the different steps we went through during the project: 
a)	preprocess BugSwarm data to adapt it to BugSwarm Commits. 
b)	train CC2Vec model on BugSwarm Commits.
c)	extract vector representation of Code Change using CC2Vec model. 
d)	Use vectors extracted by CC2Vec as inputs for build classification using supervised ML algorithms

The different steps we went through during the project are implemented in the notebooks included in this repository. For further information about the notebooks, read the documentation of code below. 

## Code Documentation ## 

### CC2Vec_Data_Visualization.ipynb ### 

#### get_diff() : ####
We use the get_diff() function in order to obtain the representation of a commit based on the repository and the commit we pass as inputs to the function. Then inside our 
function we split the commit into files and then for each file we have two lists, one for added code and one for removed code. 

#### Data creation : ####
We  first call the bugswarm api filter, then we parse the data we have in BugSwarm so that it works with CC2Vec using the get_diff() function. We add other information related 
to the commit so that it can be identical to the CC2Vec data.

#### Training the model : #### 
In order to train the CC2Vec model on our dataset, you should first go to the CC2Vec and execute the following command: 

``python jit_cc2ftr.py -train -train_data ../bugswarm_beta2.0_training_dataset -test_data ../bugswarm_beta2.0_training_dataset -dictionary_data ../data+model/data/jit/qt_dict.pkl``

#### Problems encountered : #### 
While training the CC2Vec model on our dataset we had an issue with the CC2Vec code related to array shapes. As our dataset was identical to CC2Vec dataset when visualizing it,
we estimated that the error should be related to a parsing in the command we executed. Thus, we decided to integrate CC2Vec code directly inside our code so that we manage 
to isolate the problem. This notebook is mainly proposed by Florent. Then we tried to work all of us on it in order to solve the issue related to the adaptation of our dataset to CC2Vec. 

### trainCC2VecOnBugSwarm.ipynb ### 
By following the different steps in the notebook, users would be able to train CC2Vec model on BugSwarm dataset using CC2Vec dictionary. 

### Classification.ipynb ###
In this notebook, we created 3 classifiers to predict Build failures based on the vector presentation obtained in trainCC2VecOnBugSwarm.ipynb. 
Our classifiers weren't working properly because of issues related to the vector representation returned by CC2Vec. After investigating the issue, we figured out that the problem is mainly related to the dictionary as there are many null tokens when trying to map BugSwarm Code in the dictionary. 
Thus, we created in the Classification.ipynb a new dictionary based on BugSwarm Code and retrained CC2Vec using our new dictionary. 
An error was returned in the vectors extraction step due to a mismatching between the expected input from the neural network and the size of the dictionary. CC2Vec HRNN was implemented using static fix input size. After resizing our dictionary, we had an issue with the remote server we are working on.  
