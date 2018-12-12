# ASR_2018_T01

## Data pre processing

Ran command ```python import_timit.py --timit=./TIMIT --preprocessed=False``` to compute the features and stored them in a folder.

Imported packages like numpy, pandas, joblibs, Label Encoder and Gaussian Mixture.

Training of Data:

Read the training data and stored into data frames of mfcc, mfcc_delta and mfcc_delta_delta .(phone-level segmentation)

Created a function name preprocess in which arguments were data frames ,in which taken out features and label column into training varibles
by creating list because GMM needs sequence as an argument.
After that encoded the labels using Label Encoder (label encoding for phonemes).
After that dropped column of labels(phonemes).

Created a function name remove coefficient to remove corresponding coefficient on preprocessed data frame, in which arguments are data frames and variable to compute operation.
First converting features column to a list and then to a numpy array.
Then perform if else condition and execute np.delete to remove particular coefficient from the data frame, in which variable=0 for simple mfcc,
variable=1 for mfcc_delta and variable=2 for mfcc_delta_delta.
Then put back the column of features after removing co-efficient into data frames.

Now execute preprocess and coefficient removal function created respected data frames with and without energy coefficient.

Now measured length of feature from any dataframe to verify the coefficient is removed or not.

Take out the list of labels into unique labels.

Now create function to execute models of mffc, mfcc_delta and mfcc_delta_delta for with and without coefficients (6 data frame with features)
Everything with 64 components except mfcc without variant with 2 to 256.
Function takes data frame and list for no of components.
According to data frame number created directory. Usage of if-else condition is here.
The function train the models and store it for every data frame for component list.
Then Iterate through component list for n component GMM.
For Each phoneme Iterate for unique labels and start training the components.
Applied Gaussian Mixture models on the n component and put covariance type as diagonal.
Fit the model and store into the directory if path does not exist then create the directory and store the pkl files.

Testing of Data:

For testing also need to import packages

Then take trained data into data frames.

Create test variables for features and labels.

Create function of "preprocess" and "remove coefficient" and perform same action as done in Training of data.

Take out the list of labels into unique labels.

Now import "joblibs" and "accuracy_score" pack.

Then define test models and take out features and labels column for test set.

Then create list of list which contains all 40 rows of length test data frame and then calculated predicted probability of each phoneme trained.

For each phoneme load corresponding model and predict.
To calculate probability apply score sample on loaded models of test data frame.
Done this for 40 models and apply MAP rule to get classification result at frame level.
Then appended probability list to create list of list.

Then calculate Accuracy of list of lists using "as array"
Determine maximum value column wise and we need index of it to map it to phoneme / class.

Then printed the Frame Level Accuracy for a given test feature set which is shown below.(which ran on 40 models)
--------------------------------------------------------------------------------
|Model            |  with coefficient Accuracy  | without coefficient Accuracy |
--------------------------------------------------------------------------------
|                 |                             |                              |
|mfcc             | 0.1428242483283886          | 0.13285214541912058          |
|                 |                             |                              |
|mfcc_delta       | 0.18845813222335386         | 0.17702696718770758          |
|                 |                             |                              |
|mfcc_delta_delta | 0.1930810786875083          | 0.17410662888013106          |
--------------------------------------------------------------------------------

Phoneme Error Rate:(PER) for given Utterance :

----------------------------------------------------------------------------------------------------------------------------------------------
|Model                                         |  Sentence Count |   WER(Word Error Rate)  |         WRR            |SER(Sentence Error Rate)|
----------------------------------------------------------------------------------------------------------------------------------------------
|                                              |                 |                         |                        |                       |
|mfcc (With Energy coefficient)                |      1680       | 85.383 %(385640/451660) | 15.027 %(67869/451660) | 100 %(1680/1680)      |
|                                              |                 |                         |                        |                       |
|mfcc (Without Energy coefficient)             |      1680       | 86.268 %(389638/451660) | 14.219 %(64220/451660) | 100 %(1680/1680)      |
|                                              |                 |						   |						|	            		|			|
|mfcc_delta (With Energy coefficient)          |      1680       | 80.887 %(365332/451660) | 19.408 %(87657/451660) | 100 %(1680/1680)  	|
|                                              |                 |				           |				        |					    |      	|
|mfcc_delta (Without Energy coefficient)       |      1680       | 81.957 %(370166/451660) | 18.405 %(83127/451660) | 100 %(1680/1680)	   	|
|                                              |    			 |						   |					    |						|           |
|mfcc_delta_delta (With Energy coefficient)    |      1680       | 80.443 %(363330/451660) | 19.831 %(89570/451660) | 100 %(1680/1680)		|
|                                              |    		     |						   |					    |					    |        	|
|mfcc_delta_delta (Without Energy coefficient) |      1680       | 82.293 %(371683/451660) | 18.016 %(81370/451660) | 100 %(1680/1680)	   	|
----------------------------------------------------------------------------------------------------------------------------------------------

