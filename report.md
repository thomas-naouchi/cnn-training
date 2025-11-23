# Report

This exercise consists of training a convolutional neural network on a dataset of image/artist pairs to predict the artist given an image

## Initial approach

1. Import the data from google drive and store all paintings of the same artist in a numpy array
   - Optional : shuffle each artist array
2. For each artist array, train-test split :
   - 75% train
   - 25% test
3. Merge all the train arrays and test arrays together, respectively
4. WITHOUT ALTERING THE ORDER, split training and testing into
   - x_train/test (image)
   - y_train/test (artist label)
5. Train different models (check for pre-trained models) on different sets of hyperparameters and plot all results
6. Using the parameters that yielded the best results, repeat ~10 times :
   - shuffle the data RANDOMLY
   - Split into train/test
   - train + test to make sure the results were not randomly the best ones
   - Store all metrics for later use
7. Get the average of all stored metrics + std deviation
8. Using these same parameters, train a new model on all the dataset and save it

## Deeper dive in technicalities

###### importing the data in numpy arrays:

27 arrays because we have 27 artists
Each array will be named by an artist name and contain the images of the corresponding artist
In a sorted manner, for reproducibility of the model, i stored the images of each artist in a different array, then stored this array in a dictionary

###### preparing train and test sets:

In the dictionary {artist : [images]}, i transformed each image array to a numpy array for better manipulation
I shuffled all the images in place using a seed for reproducibility
75% of the images went for training, the other 25% for testing
Train and test sets are numpy arrays of [image, label] pairs, which will make it easier to divide them into X_train, y_train, X_test, y_test
X_train and X_test images were normalized by changing the type to 'float32' and dividing each value by 255
y_train and y_test artist names were encoded using sklearn.preprocessing
Small thing i fixed :

- shuffle all the training set before dividing into X_train, y_train
- shuffle all the testing set before dividing into X_test, y_test
- because shuffling the initial arrays will only define which of the data will go to training and testing, not the order in which it will be fed to the neural network. we need to change this order so not all images that have the same artist appear in the same batch

###### cnn training

After trying everything and always getting validation accuracy of 0.2326, i decided to implement data augmentation to "add" more images to my dataset.
I then randomly stumbled across "tf.keras.preprocessing.image_dataset_from_directory()" documentation, and realized what went wrong and why my algorithm was so slow
Importing the data was done in a single run using OpenCV, then when all data is imported, i move on to model training.
Even when using a GPU or a TPU, training still took a lot of time, and this was because the dataset would pass the images during training image by image, instead of batches of multiple image, so my model was implemented in a way that runs sequentially.

##### Different dataset-import approach

###### tf.keras.preprocessing.image_dataset_from_directory()

I realized that importing a dataset that way was much quicker and easier.
Turns out the dataset was not imported, but would actually be imported whenever needed, ex: train the model, print or display the dataset.
So instead of having a sequential (import all data -> train the model image by image) flow, the new flow was (whenever i want to train, import a batch of 32 images -> pass this batch for training -> repeat importing and passing data until all data is imported)
This would solve an issue i used to face earlier, as my RAM would be flooded with data and at some point even become full and i would have to rerun everything on colab hoping to not run out of memory again

###### preparing train and test sets

Imported 75% of data for training and 25% for testing

######
