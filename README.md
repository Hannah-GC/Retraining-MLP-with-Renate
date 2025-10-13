# Introduction
The initial stage of training a new machine learning model is relatively straightforward.  Data is given to the model, and it learns the weights which will best minimize its loss and maximize its accuracy.  How well the model generalizes to new data can be measured through evaluating it on a testing set (and sometimes, a validation set during training).  However, in practice, the training and testing data will not be the only data that the model sees.  New data will always be made available, and it is important that the model is kept up to date to ensure that its predictions remain relevant to current data.  Therefore, finding out how to re-train models in an efficient manner while maintaining its knowledge of previously learned information is quite important.

When it comes to the “easy” ways of fine-tuning models, a lot of them are usually inadequate.  One approach is to simply reload the trained weights of the model, and conduct the exact same training process, except with the new data.  This can easily lead to catastrophic forgetting, where the model loses most of the information it already learned while being trained on a new task.  Another approach is to start over from scratch, instantiating the model again with untrained weights, and go through the training process again, except that the old and new datasets are concatenated together to make new training data.  This is largely inefficient, since virtually none of the work put into creating the existing model is re-used to speed up the process.

# What is Renate?
Renate is a Python package which can be used for retraining a deep learning model.  It was developed by Amazon Web Services, and was made public around December 2022.  It provides a method of continual learning which tries to avoid issues that may come up during traditional fine-tuning methods, such as catastrophic forgetting.  Additionally, Renate includes other functionalities such as options for hyperparameter optimization (HPO).  This project aims to use Renate to demonstrate an efficient way of fine-tuning model parameters.

# How can Renate be Used?
The intended way of using Renate is to define model parameters and settings within a separate configuration file, and re-train using the run_training_job() function.  However, it is also possible to use Renate in a simpler manner, defining a data module and model updater within one file.  For this project, the latter method is used.  For more information on how to use Renate, see [Renate Documentation](https://renate.readthedocs.io/en/latest/index.html#cite-renate) for more information.

# Project
The project involves a basic MLP model with tabular data, and aims to simply compare the performance of a model re-trained through traditional means, and a model re-trained using Renate.  In this case, the traditional method of re-training the model refers to performing the initial training, saving the weights, reloading them, and using the new model with the pre-trained weights to perform another training round on the "new" data.  The flow of pre-processing the data, training, re-training, and using Renate can be followed in the code file.

# Results Summary
While the results were not ideal, they do show a noticeable decrease in loss and increase in accuracy when using Renate to re-train the model.  Below is the comparison between all metrics.  Note that Renate did not output training and validation accuracy, but test accuracy was obtained through reloading the model weights.

| Stage | Train Loss | Train Accuracy | Val Loss | Val Accuracy | Test Loss | Test Accuracy |
|------------------|------|----------|-----------|------|----------|----------|
| 1st Round | 2.0643 | 0.2567 | 2.0600 | 0.2598 | 2.0615 | 0.2588 |
| 1st Re-Train | 2.0884 | 0.2355 | 2.0920 | 0.2358 | 2.0877 | 0.2382 |
| Renate Re-Train | 1.6071 | Not Available | 1.6060 | Not Available | 1.6100 | 0.4009 |

# Future Applications
Renate does seem to have good potential for being used in fine-tuning models, at least for certain models and/or data.  More work should be done to accurately determine Renate’s limitations, and/or develop it further to be compatible with larger models and datasets.  Additionally, future experiments could involve exploring other functionalities included in Renate, such as different updaters and different benchmark models.
