# Protein-Family-Classification
Abstract:

Protein classification is a huge area of study in bioinformatics and computational biomedical engineering. Knowing the family of a protein makes it much easier to predict its classification. Predicting protein classification is vital in the life sciences for the study of new diseases, drug development and biomaterials. In recent years, unsupervised learning has emerged as a powerful tool to help solve previously difficult problems involving large and complex datasets. We developed a novel deep learning algorithm which can classify proteins by family using only their sequences. By applying natural language processing to the sequences in order to represent them as numerical vectors, we were able to utilize a convolutional neural network in order to successfully classify 750 different protein families using over one million training examples from the pfam database. This model was able to achieve an accuracy score of 99.8 percent. This model can be used to reliably and efficiently predict the family of unknown protein sequences.

Materials / Methods:

The dataset contains one hundred files split into three different directories. 80 percent are in the train directory, and 10 percent are in the val and test directories. This dataset contains information from the PFam database. It contains 18,000 unique output classes(protein families) and over 1 million training examples. We read and concatenated each file into one large pandas dataframe for each directory. Each data frame contained the dataset was already split into a train, val, and test split when imported. The val and train test splits are necessary to prevent our model from overfitting. Overfitting occurs when a model memorizes the unique qualities about the dataset, however does not create any basic principles that can be applied to future datasets. These val and train test splits allow for our program to be tested on new data it hasn’t yet trained on, so that we can determine whether or not the model we have created is accurate.

We found that a very small amount of proteins had missing values, and decided that it was best to outright remove these values from our train, test, and validation splits to create a more accurate model. Had we have more missing values, we may have chosen instead to replace those values.

Due to hardware limitations, we had to limit our output classes to 750. We had run many tests with more than 750 output classes, however found that we either had insufficient random-access-memory (RAM), or our program would crash for taking too long to train the model. We chose not to downsample any further after running 750 classes to maximize the accuracy of our model.

We chose to use sequences to predict classifications based off of their sequences because we found that sequences are the most unique part of a protein, and that proteins are best distinguished from one another based off of their sequences, allowing for the most accurate classification prediction. There were many other variables that we could have used to train our model such as residue count, density (Matthews), and crystallization temperature. However, the model cannot train with both the sequences and these numerical variables. This is due to the fact that the sequences are converted into vectorized data, while numeric variables cannot. Therefore, we chose to only use sequences.

The problem of sequence classification is similar to sentence classification in Natural Language Processing. Therefore, we decided to treat sequences as an unknown language and applied the tokenizer tool from keras. First, we fit the tokenizer on our text. The tokenizer updates its internal vocabulary based on a list of text(in our case sequence). Therefore, when reading a sequence the tokenizer is able to detect unique patterns and assign corresponding integer values to these patterns. The more frequent patterns are represented by lower integers. This can be better imagined with one representing the most frequent pattern, two representing the second most frequent pattern and so on. Once the tokenizer has trained on the sequences, we now have a word index which we can use to convert the acid sequences into corresponding integers. After applying this word index to each sequence we now have strings of numerical vectors instead of amino acid sequences. This is critical because machine learning models are only able to be trained on numerical vectors. One important note is that we limited the length of each sequence to a maximum length of 128. This served two purposes. One, it created a certain degree of standardization to prevent overfitting or incorrect weighting. Second, since each sequence consists of the same 20 characters, the probability of sequences containing similar patterns increases greatly as the sequences become longer. By examining only the start of sequences, we increase the probability that we see noticeable differences. In short, the idea is that proteins from the same family will have highly similar sequences and those from different families will have certain differences. Using powerful machine learning models, we can easily spot these unique patterns and use them to easily classify proteins.

We also built a Long-Short Term model, a special kind of Recurrent Neural Network. A Recurrent Neural Network (RNN) is a model that makes use of sequential information. This differs from a traditional neural network which assumes that all inputs and outputs are independent of each other. RNNs perform the same task for every element of a sequence, resulting in an output that is dependent on previous computations. This makes RNNs great for many natural language processing tasks. The major problem with RNNs however is that they can only make accurate models based on recent training features. Here is where a Long-Short Term model is proposed. Introduced by Hochreiter & Schmidhuber in 1997, Long Short Term Memory networks (LSTMs) are designed to remember information for long periods of time. LSTMs work by having a chain of repeating modules of neural networks.Introduced by Hochreiter & Schmidhuber in 1997, Long Short Term Memory networks (LSTMs) are designed to remember information for long periods of time. LSTMs work by having a chain of repeating modules of neural networks. The main part of LSTMs is the cell state, which is the horizontal line running through the top of the module. The cell state allows for information to pass through the modules unchanged. To add or remove information from the cell state, gates (composed out of a sigmoid neural network layer and a pointwise multiplication operation) outputs a number between 0 and 1 to decide how much new information should be let through. In each module. there are three gates. The first gate, called a "forget gate layer" marked by a sigmoid, decides which information to completely keep or completely forget. The second gate has two parts. The first part, a sigmoid layer, chooses which values to update. The next part, a tanh layer, creates a vector of new values that may be added to the cell state. Next, the module carries out the decisions made in the previous two gates. This includes deciding which information should be kept or dropped, and adding new values that are scaled to how much of that value should be added. The module then runs a final sigmoid gate to decide which part of the cell state to output. The cell state then goes through tanh (which pushes values between -1 and 1) and multiplies it by the output of the sigmoid gate, so that we only output the parts we want.

Within our LSTM model, we used six unique layers. The first layer, an embedding layer, converts the sequences into embedding vectors that our model can use.The first argument is the size of the vocabulary, and the next is the size of the resultant vector you want. The next argument specifies the length of each input. Our second layer is a masking layer, which is a way to tell our LSTM layers that certain timesteps in an input are missing, and should be skipped when processing the data. Our third layer is our first LSTM layer. The argument within this layer specifies how many cells should be in each of the three sigmoid gates. (Not too sure what the next couple of arguments do). We next added a dense layer, inputting more features into the program. A dropout layer was then added, preventing our model from overfitting, and ultimately letting our model make accurate predictions on a relatively small dataset. A final dense layer was added before our model was finally compiled. 



