## Following things were tried for the assignment

* Tried 5 class as well as 25 class classification. To convert the data into 25 class variable - change variable (v_range). 
* The accuracy in case of 25 class classification, started with 24 % and similarly reduced to 19% after 30 epochs
* Accuracy on validation set has reduced from 45% to below 40% in case of 5 class classification
* Data Augmentation was performed using random_insertion with synonyms as well as random_swap. None of these helped in the model
* Also changed the model to pass only last hidden layer to fully connected layer but did not help the model
* Tried changing the learrning rate few times but that also did not help the model
* Tried GRU as well but that also did not help the model
* Looks like something is completely ignored and hence the model is getting to overfitting with more training
