# Report

This exercise consists of training a convolutional neural network on a dataset of image/artist pairs to predict the artist given an image

## Initial tentative
1. Import the data from google drive and store all paintings of the same artist in a numpy array
    -   Optional : shuffle each artist array
2. For each artist array, train-test split :
    -   75% train
    -   25% test
3. Merge all the train arrays and test arrays together, respectively
4. WITHOUT ALTERING THE ORDER, split training and testing into
    -   x_train/test (image)
    -   y_train/test (artist label)
5. Train different models (check for pre-trained models) on different sets of hyperparameters and plot all results
6. Using the parameters that yielded the best results, repeat ~10 times :
    -   shuffle the data RANDOMLY
    -   Split into train/test
    -   train + test to make sure the results were not randomly the best ones
    -   Store all metrics for later use
7. Get the average of all stored metrics + std deviation
8. Using these same parameters, train a new model on all the dataset and save it 

