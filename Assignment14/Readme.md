## Capstone Project - English to Python

The capstone project to train a model to convert a comment to python code was attempted in this assignment. The assignment following following steps

# 1. Pre-processing

The dataset shared was a single file with comments and code in it. As part of pre-processing the code was converted into a source and target dataset. Source is the comments from the given text file and target is the source code against the given comments. During the pre-processing following challenges were faced

1. The data in the file did not have proper line spacing between the different codes and comments. So, as a start step, any line starting with # is considered as code and all other lines below the comment are considered as code (until the next comment is hit).
2. The lines which had just the Spacing i.e. new line without any characters are removed.
3. Few places in the datafile, only the comments were found continuously in 7-8 lines and such things are cleaned manually
4. In certain cases, source code had comments at the line level (after the program level comments). In case the comment starts with space i.e. "    # Comment" is considered as part of the code while the comment starting with # at first characted in line breaks the code into next source and destination. This is not expected as per the datafile given, howvever I included this as acceptable noise in data. The pre-processing logic would have complicated in absence of this assumption.
5. The # from the comments is removed while storing the source into training data
6. In certain cases, target length was going upto 1500 words i.e. the source code against the comment had more than 1500 words. It was causing a big problem while training and hence any target with more than 450 characted was pruned. The max length for encoder and decoder classes was defined as 500 keeping this limit in mind. This also required adjusting the batch size and fixed at 64.
7. Though the comment or source did not have huge length but this was also fixed at 450 words during pre-processing.

# 2. Model

There was no change in the model from the assignment 12 and assignment 13. The model in those assignments are duly explained with all the salient features. The same model (exactly the same code) was used in this assignment as well. Please refer to the link below for assignment 12

https://github.com/atulgupta01/END/blob/main/Assignment12/Assignment_12.ipynb

# 3. Loss Function

This was quite an interesting assignment where it is important to generate the code with exactly the same words (in same sequence). The prediction for normal language translation can have swapping of words or even different words with same meaning. However, this is not possible in this case. Hence Entropy loss was the loss function selected and did not try any other loss function

# 4. Data Preparation

As explained in the pre-processing step, data was converted into a pandas data frame. Then it was converted into examples for converting into the dataset for pytorch. The tokenization was performed using SPACY for source as well as target. The data was divided into train, validate and test as 80%, 10% and 10%.

# 5. Data Extension

Back translation of the source (comments) is adopted as a data extension strategy. Selected approx 1700 lines randomly and reversed those strings by words (not character) and added as additional data to the dataset. The target remains the same for the back translated source. This helped in reducing the validation loss and perplexity drastically

# 6. Python Code Embedding

There was not special strategy adopted for the code embedding except each token was considered for embedding i.e. min_freq was set to 1. Ideally wanted to try a strategy where "X(" and "X (" are treated as two words "X" and "(". Similarly "X:" and "X :" should be converted into "X" and ":". But could not find an efficent way of doing it and hence discarded the option.

# 7. Evaluation Metrics

Used loss and perplexity as evaluation metrics. It was observered that the decrease in loss for validation data resulted in decrease in perplexity. Also, the out of the prediction was much better. The output of prediction was looked with following objectives

1. Completion of the code or function
2. Proper indentation of the code
3. Code gets compiled in the compiler without errors
4. Expected code from the comment

# 8. Example Codes

The folder has two files "train_example.txt" generated for the training dataset and "test_example.txt" generated on the test dataset. 

The **comment** represents the source, **Actual Code** represents the ground truth or the target data used for training and **Predicted Code** represents the generated code by the model

**Data.csv** is the file generated and used for training after all the pre-processing etc.

# 9. Graphs

Graphs are generated and shown in the python notebook.

# 10. Additional points

1. The training was performed for 40 epochs but usually around 20 epochs, best model was achieved. So rest of the training was not reducing the validation loss or perplexity. 
**Lesson** - hyperparameter tuning should be learned in detail

2. Some of the output were interested i.e. not matching with the actual code but still the out was correct. An example is given below

# Comment

write a python function to that performs as relu

# Actual Code

def relu(num ) : 

    if num > 0 : 
    
        return num 
        
    return 0

# Predicted Code

def relu(num ) : 

    if num > 0 : 
    
        return num

3. There are some predicted codes which were generated and not compilable

4. There are some predicted codes which are compilable but do not give the expected output 
