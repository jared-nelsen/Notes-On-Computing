# Hands on Machine Learning with Scikit-Learn and Tensorflow

----------------------------------------------------------------------------------------------------------------------------------------------------------------

## Chapter 1: The Machine Learning Landscape

## The Landscape

Machine Learning is the science of programming machines so that they can ***learn from data***. It is the study that gives computers the ability to learn without explicit programming. Machine Learning can be used to dig into large amounts of data to learn the patterns therein: this is called data mining.

Machine Learning is great for:

1. Problems for which existing solutions require a lot of hand tuning or long lists of rules.
2. Complex problems for which there is no known solution.
3. Fluctuating environments.
4. Getting insights into large amounts of data.

#### Supervised Learning

In supervised learning, the training data you feed to the algorithm includes the ***desired solutions called labels***. ***Classification*** and **Regression*** are examples of ***supervised learning***. To train the system, you need a large set of *predictors* and *labels* to pass to the algorithm. 

Some supervised learning algorithms include:

1. K Nearest Neighbors
2. Linear Regression
3. Logistic Regression
4. Support Vector Machines
5. Decision Trees and Random Forests
6. Neural Networks

#### Unsupervised Learning

In **unsupervised learning**, the training data is ***unlabeled***. The system tries to learn without a teacher.

Some unsupervised learning algorithms are:

1. Clustering
   1. K Means
   2. Expectation Maximization
2. Visualization and dimensionality reduction
   1. Principal Component Analysis
   2. Locally Linear Embedding
3. Association Rule learning
   1. Apriori
   2. Eclat

***Dimensionality Reduction*** is the practice of simplifying data without losing too much information. One way to do this is to merge several correlated data features into one. This is called ***Feature Extraction***. Two other unsupervised tasks are ***Anomaly Detection*** ***Association Rule Learning***.

#### Reinforcement Learning

Reinforcement Learning is a little different. In this case, an ***Agent*** performs actions and receives ***Rewards*** and ***Penalties*** based on performance. Minimizing a ***Cost Function*** over time achieves learning. 

#### Batch Learning

In batch learning, the system is trained on ***all available data***. In a practical sense, this means that systems are trained in isolation from potential new data. The model is trained once and then used: there is ***no active learning***.

#### Online Learning

In online learning, you train the system ***incrementally*** by feeding it data instances sequentially. This method allows a model to be trained rapidly, autonomously, and continuously. The system ***learns actively***.

#### Instance Based Learning

***Generalization*** is the ability for a system to accurately predict or classify samples that it has never seen before. Instance based learning is one approach to achieve this.

Instance based learning requires a ***measure of similarity*** between two samples. Identical samples are just that: identical. Samples that are very similar but not exactly the same have a high degree of similarity and visa versa. The system ***learns by heart*** and generalizes to new cases using a similarity measure.

#### Model Based Training

Another way to generalize between examples is to ***build a model*** and use it to ***make predictions***. You can then ***select the best model*** to represent your data. If your data is a simple correlation, a linear function might suffice. ***Training the model*** to make it represent the data the best using reinforcement learning allows you to ***represent the data as a class*** and use that representation to make future classifications. The pattern goes ***study*** the data, ***select a model***, ***train the model***, and then to ***use the model***.

## Main Challenges in Machine Learning

#### Insufficient Quantity of Training Data

Machine Learning takes a lot of data. Especially if your task is non-trivial, it may take millions of examples.

#### Non-Representative Training Data

In order to generalize, your sample data needs to be representative of the data you are trying to model. If your sample size is ***too small*** you will have ***sampling noise***. Even if your sample size is very ***large***, it doesnt guarantee that it is representative. This is called ***sampling bias***.

#### Poor Data Quality

Models will not generalize if the training data is full of errors, outliers, or noise. Sometimes it is worth the effort to clean up data or fix it manually somehow. You can ignore data or attributes.

#### Irrelevant Data

Feature engineering is the process of determining what data is relevant. The can be done using ***feature selection***, selecting the most useful features to train on among existing features, and ***feature extraction***, combining existing features to produce a more useful one.

#### Overfitting

Humans have a tendency to over generalize based on a single or relatively few events. If the training set is too noisy, it is possible for a model to detect patterns in the noise and not the data. 

***Overfitting happens when*** the model is too complex relative to the amount and noisiness of the training data. Possible solutions are:

1. Simplify your model by selecting one with fewer parameters, reducing the number of attributes in the training data, or by constraining the model.
2. To gather more training data.
3. To reduce the noise in the training data.

Constraining a model to make it simpler and reduce risk of overfitting is called ***regularization***. The amount of regularization to be applied during learning can be controlled by a ***hyperparameter***. Hyperparameters are set before training and remain constant during.

#### Underfitting

***Underfitting*** is the result of a model that is so simple that it fails to learn the underlying structure of the data. An example might be a linear model trying to approximate a learning rule on a data set that is best represented by a polynomial.

The main options to fix underfitting are:

1. Selecting a more powerful model with more parameters.
2. Feeding better features to the learning model.
3. Reducing the constraints on the model (Including hyperparameters)

## Testing and Validating

The only way to know how well a model will generalize to new cases is to actually try it out on new cases. You can do this by splitting your training set into two sets: the ***training set*** and the ***test set***. You can train on the training set and test on the test set. The error rate on the new cases is called to ***generalization error*** which will give you a measure of how well your model will perform on novel samples. A good rule is to use 80% of data to train and 20% of data to test.

## Chapter 2: End to End Machine Learning Project

A ***general plan to a machine learning project:***

1. Look at the big picture.
2. Get the data.
3. Discover and visualize the data to gain insights.
4. Prepare the data for Machine Learning algorithms.
5. Select a model and train it.
6. Fine-tune your model.
7. Present your solution.
8. Launch, monitor, and maintain your system.

## Chapter 3: Classification

## Binary Classifiers

We've seen several examples of binary classification algorithms: Support Vector Machines and Linear classifiers.

### Confusion Matrix

A confusion matrix is a good was to evaluate a classifier. The general idea is to count the number of times instances of class A are confused and classified as class B.

### Confusion classifications:

1. ***Negative Class:*** The class of data that are non-class. In other words, if we have a binary classifier, these are the samples that are true negatives.
2. ***True Negatives:*** Classifications that were correctly classified as as the non-class.
3. ***False Positives:*** Classifications that were incorrectly classified as the positive class.
4. ***Positive class:*** The class of data that is the true-class. In other words, if we have a binary classifier, these are the samples that are true positives.
5. ***False Negatives:*** Wrongly classified as a member of the non-class.
6. ***True Positives:*** Correctly classified as the true class.

## Multivariate Classifiers

***Multivariate Classifiers*** are classifiers that can distinguish between two or more classes. Some examples are Random Forest classifiers or Naive Bayes classifiers.

Binary classifiers can be combined to perform the job of multivariate classifiers:

1. ***One versus all:*** Train many Binary classifiers, one for each class. When you classify a sample you get the selection score for each and take the highest of the set.
2. ***One versus one:*** Train Binary classifiers to distinguish between all pairs of classes. For instance, if you are classifying handwriting digits from 0 to 9 then you must create a classifier to distinguish between 0 and 1 and 1 and 3 and 0 and 3 etc....

## Chapter 4: Training Models

### The Bias Variance Tradeoff

A model's generalization error can be expressed as the sum of three types of error terms:

1. ***Bias:*** Bias is due to wrong assumptions such as assuming the data is linear when it is actually quadratic. A high bias model is most likely to underfit training data.
2. ***Variance:*** Variance is due to a model's excessive sensitivity to small variations in the training data. A model with many degrees of freedom is likely to have high variance and thus will overfit the data.
3. ***Irreducible Error:*** This is due to the noisiness of the data itself. The only way to reduce it is to clean the data somehow.

Increasing a model's complexity will typically increase its variance and reduce its bias. Conversely, reducing a model's complexity increases its bias and reduces its variance.

### Regularized Linear Models

The more degrees of freedom a model has, the easier it is to overfit the data. To correct this, we can ***regularize*** the model. For example, to reduce the degrees of freedom of a polynomial regressor just reduce the degree of the polynomial. For linear models, regularization is achieved by constraining the weights of the model using these techniques:

1. Ridge Regression - A regularization term is added to the cost function: A*sum(1 -> n)phi (sub i) squared. Once the model is trained you should remove the regularization term.
2. Lasso Regression - Lasso regression is like ridge regression: it uses a regularization term in the cost function to constrain the model. The equation is on page 130. Lasso regression eliminates the weights of the least important features: it turns them to zero transforming the model into a sparse model with few nonzero weight features.
3. Elastic Net - A middle ground between Ridge Regression and Lasso Regression. You have a term that ranges from pure Ridge Regression to pure Lasso Regression called r. r can be manipulated to gain the right constraint term between the two types.

## Logistic Regression

Logistic Regression ***estimates the probability*** that an instance belongs to a particular class. It is a binary classifier. It uses the logistic function to create a range between 0 and 1. The logistic nature of the function smoothes the probability distribution between 0 and 1 to create the classifier. The objective of the training is to set the parameter vector phi so that the model estimates ***high probabilities for positive instances*** (y = 1) and ***low probabilities for negative instances*** (y = 0). On either side of the sigmoidal function, the curve acts as a logarithmic curve where the extremes are of a large absolute value. 

## Chapter 5: Support Vector Machines

### Linear SVMs

Support Vector Machine is a very powerful model that is capable of ***linear and nonlinear classification, regression, and outlier detection***. You can think of SVMs as fitting the ***widest possible street between the classes***. This is called large margin classification. SVMs are sensitive to outliers: they can be altered by just one or two odd pieces of data. The objective is to find a good balance between keeping the street as large as possible and limiting margin violations. The ability to generalize is preferred over model accuracy. This balance can be tuned using hyperparameters. Note that just because an SVM is wide, doesn't mean it is accurate.

One problem is that most models are not linearly separable. Features can be introduced to separate the data. Data that has been transformed by a function is just as valid as the original data. For instance, say we have two classes A and B that just happen to fall on a line. To transform the data, apply a function: x2 = (x1)^2. Now the data is separable.

Converting an SVM to use polynomials is a great way to increase accuracy but be careful to avoid overfitting and underfitting.

### SVM Regression

Recall that SVMs can support classification and regression. To perform regression instead of classification reverse the objective: instead of trying to fit the largest possible street between two classes, try to fit as many instances as possible ***on the street***. Again, the width of the street is controlled by a hyperparameter. 

### SVMs: Under the Hood

The SVM classifier predicts the class of a new object simply by computing the decision function W^T * x + b = w1 * x1 + ... wn * xn + b where b is the bias term. If the result is positive then the predicted class is the positive class and if it is negative then the predicted class is the negative class. The dashed lines in the pictogram show where the decision function's boundaries are: -1 and 1. The actual decision boundary is the middle line. The goal is to widen the margin between the dashed lines such that margin violations are minimized.

## Chapter 6: Decision Trees

Decisions trees are ***capable of both classification and regression tasks.*** Decision trees are able to fit very complex data sets. They are the basis for ***Random Forests*** which are among the most powerful algorithms.

### Decision Tree Classification

The underpinnings of decision trees are rather simple. Imagine a tree with one left node, one right node and the right node having both a left node and a right node. Each of these nodes has a real value that indicates some feature about the tree. Each node also has a class associated with it. To determine a class, simply ***traverse the tree with the real valued number and the class will be what node it ends up at.*** Decision trees have the tendency to overfit. ***Non-parametric models*** are models that are allowed to fit the data however it chooses. ***Parametric models*** are constrained by predetermined parameters: they allow the fit to be more general through a greater degree of control. Again, constraining the degrees of freedom in a training set is called ***regularization.*** In decision trees, regularization simply looks to constrain the model by limiting the depth at which the tree can grow. By doing so, the number of classes is mapped to a larger range of the real valued decision interval. 

### Decision Tree Regression

When a Decision Tree is used to perform regression, the difference is that instead of predicting a class for each node, ***we predict a value.*** Notice that the predicted value for each region is always the average target value of the instances in that region.

### Instability

Decision trees have a few limitations. First, Decision trees ***love orthogonal decision boundaries*** which makes them sensitive to the orientation of the class data within a plane. Even if a model fits a decision boundary such that it is theoretically perfect, it may be that it will not generalize well. One of the keys to generalization is to make a decision boundary that is non-complex and is not tightly bound to the data. ***The further away from a decision boundary the data sets lie, the better it will generalize.*** This is because when a new data point is introduced, it will likely fall on the right side of the decision boundary. This is why SVMs are powerful: they try to make the widest possible ***decision field*** between the classes. 

## Chapter 7: Ensemble Learning and Random Forests

### Introduction to Ensemble Learning

Suppose you ask a complex question to 1000 people. The combined answer will likely be better than an answer that is given by a single individual. In the same way, if you aggregate a group of predictors you will get better predictors than a single predictor. You can train a group of Decision Trees on differing random subsets of the training set. Then to make predictions, obtain the predictions of all individual trees and then predict the class with the most votes. This is called a ***Random Forest.*** The method of using an ensemble of predictors can be extended to use disparate types of predictors: you could have a logistic regressor, an SVM, and a K - Nearest Neighbors. These could be considered ***weak learners*** whereas their combination into an ensemble could be considered a ***strong learner.*** An ensemble can use a ***majority vote*** method or a ***highest confidence*** method to weigh results. Ensemble methods work best when predictors are as independent from one another as possible.

### Bagging and Pasting

***Bagging*** is the practice of training the different classifiers in an ensemble on different subsets of the training data. Bagging allows the training instances to be sampled several times for each predictor. 

### Random Forests

As stated, ***Random Forest*** is an ensemble of Decision Trees generally trained via the bagging method. The Random Forest algorithm introduces extra randomness when growing trees. Instead of searching for the very best feature, it searches for the best feature among a random subset of features. The result is a greater tree diversity which in essence trades a higher bias for a lower variance. If you look at single Decision Trees, it is likely that the more important features will end up higher up in the tree. It is thus possible to rate the importance of a feature by how deep it is the the tree.

### Boosting

***Boosting*** refers to an Ensemble method that can combine several weak learners into a strong learner. The idea of boosting is to train predictors sequentially, each trying to correct its predecessor. The most popular boosting method available is ***AdaBoost.*** 

## Chapter 8: Dimensionality Reduction

***The curse of dimensionality*** refers to the fact that there may be millions of data points per training instance, making training and good model building intractable. Fortunately, oftentimes it is possible to reduce the richness of the training data to make training more feasible and still retain enough data to make a good training system. For example, oftentimes in an image two pixels that are side by side are highly correlated and can be merged without losing much information. On the other hand, too much dimensionality reduction can lead to very convoluted training systems so it must be weighed in light of training feasibility.

High dimensional datasets are likely to be very sparse. The more dimensions a training set has, the likelier it will be to overfit the data.

### Projection

In most real world problems, training instances are not spread out uniformly across all dimensions. All training instances lie within a much lower dimension ***subspace*** of a high dimensional space. In this way, higher dimensional data is easily ***projected throughout*** a lower dimensional one. The book show a great picture of a 3d set projected onto a 2d plane. 2d planes are the ideal dimension to be doing machine learning on. However, projection runs the risk of reducing the accuracy of data representation.

### Manifold Learning

A 2d ***manifold*** is a 2d shape that can be bent and twisted in a higher dimensional space. For instace, a plane in the shape of a twist. Many dimensionality reduction algorithms work by simply modeling the manifold of a dataset which is called ***manifold learning.*** However, something to keep in mind is that the decision boundary of a dataset might not always be simpler in a lower dimension.

### Principal Component Analysis

PCA is by far the most popular of the dimensionality reduction algorithms. It works by identifying the ***hyperplane*** that lies ***closest to the data*** and then ***projecting the data*** onto it. Selecting the axis in the data that ***preserves the maximum amount of variance*** is the right strategy. PCA identifies the axis that accounts for the larges amount of variance in the training set. PCA finds as many axes in the dataset as there are dimensions and chooses the one with the highest amount of variance. There is a simple matrix factorization technique called ***Singular Value Decomposition*** that decomposes the training set matrix X int the dot product of three matrices U * E * V^T where V^T contains all the components we are looking for.

--------------------------------------------------------------------------------------------------------------------------

## Neural Networks and Deep Learning

----------------------------------------------------------------------------------

## Chapter 9: Up and running with Tensorflow

A Tensorflow ***session*** takes care of placing the operations onto devices such as GPUs and CPUs and running them. Tensorflow programs are often broken up into two parts: the ***construction phase*** builds a computation graph and the subsequent ***execution phase*** runs it. Operations in Tensorflow are called ***ops*** for short.

Lots of details without context in this chapter so I will skip over it.

## Chapter 10: Introduction to Artificial Neural Networks

### Number of Neurons per Hidden Layer

Obviously, the number of neurons in the ***input and output layers*** is determined by the ***input and output your task requires.*** Unfortunately, finding the perfect amount of neurons is still somewhat of a black art. A simpler approach is to pick a model with more layers and neurons than you actually need and then use ***early stopping*** or ***dropout*** to prevent overfitting. 

### Activation Functions

In general it is best practice to use ***ReLu (Rectified Linear Units)*** in your hidden layers. It is a bit faster to compute than other activation functions. One thing to keep in mind is that the output layer activation functions can be altered to meet the needs of the problem at hand.

## Chapter 11: Training Deep Neural Nets

### Vanishing/Exploding Gradient Problems

The backpropogation algorithm works by going from the output layer to the input layer, distributing the blame for the error on the way. Unfortunately, gradients often get smaller and smaller as the algorithm progresses down to the lower layers. This is called the ***vanishing gradients problem.*** On the other hand, sometimes the gradients can get bigger and bigger so many layers get insanely large weight updates and algorithm never converges. This is called the ***exploding gradients problem*** and most commonly occurs in recurrent neural networks. It was discovered that two of the culprits were the normal distribution of 0 to 1 in the weight initialization and the sigmoidal function.

Glorot and Bengio proposed a way to avoid these problems. The idea goes: for the signal to flow properly in both the forward and backward direction the variance of the input to a given layer should be equal to the variance of the output. To achieve this we can use ***Xavier initialization*** which is to initialize the weights on both sides of the layer to: phi = sqrt(2 / numInputs + numOutputs). This is simplified and more effective if the number of inputs is equal to the number of outputs.

### Nonsaturating Activation Functions

Again, one of the insights that was uncovered was that ***poor choice of activation functions*** caused exploding/vanishing gradients. It turns out that although the sigmoidal function occurs naturally in real neurons, the ***best choice*** for activation functions is actually the ***ReLu function and its variants.*** Unfortunately, ReLu is not perfect: it suffers from a problem called ***dying ReLus***  where a large number of ReLu neurons essentially die and output nothing but 0. To solve this problem you can use the ***leaky ReLu function***. The function is: max(phi z, z) where the hyperparameter defines how much the function leaks: it is the slope of the function for z < 0 and is typically ***set to 0.01.*** In several papers it was found that ***the leaky ReLu function always outperformed the regular ReLu function.*** However, there is yet another function that performs the very best called the ***ELU function*** (see p. 280 for equation). However, because ReLu is faster to compute than ELU, you may prefer ReLu for runtime critical applications. 

### Transfer Learning

***Transfer learning*** is the practice of using layers trained in other models to decrease the intensity of the training for the given model. This can be as simple as taking layers from one trained network and using them in another. 

### Avoiding Overfitting Through Regularization

The flexibility of neural networks make them prone to overfitting. There are several techniques that can be used to avoid this.

#### Early Stopping

A simple solution to avoid overfitting is called ***early stopping*** which is the practice of interrupting training once its performance on the validation sets starts dropping. In short, networks can be continually evaluated on their validation sets and when their ability to generalize begins to drop we assume the maximum efficacy in prediction has been reached.

#### Dropout

The most popular technique for regularization in deep neural networks is ***dropout.*** The algorithm is simple: at every training step, every neuron excluding the output neurons has a probability of ***p*** of being temporarily dropped out. ***p*** is called the ***dropout rate*** and is typically set to ***50%.*** 

### Data Augmentation

Data Augmentation consists of generating new training instances from existing ones. This artificially boosts the size of the training set. Ideally, a person should not be able to tell the difference between data that was generated and the original. Also, just adding white noise wont help: you need to generate features that are learnable. One of the best examples is to generate new versions of an image by rotating it and panning it. Tensorflow offers several functions to augment images like this.

## Chapter 12: Distributing TensorFlow Across Devices and Servers

Tensorflow allows you to distribute computation across multiple devices and servers. This allows for parallel and large scale training. 

Just read through this chapter. Too many implementation details to worry about recording here.

## Chapter 13: Convolutional Neural Networks

Convolutional Neural Networks have been successfully applied to image processing tasks and to NLP.

The most important part of a CNN is the ***convolutional layer.*** Neurons in the convolutional layer are not connected to every single pixel in the input image but only to pixels in their receptive fields. This allows a sort of layering between layers where low level features are concentrated on first and complexity is built up in the hidden layers from there. 

### Filters

A neurons weights can be represented as a small image the size of the receptive field. A ***filter*** is a function that is run over an image to break it down into some constituent parts. For instance, there are color filters, shape filters, vertical filters, and horizontal filters among many others. A convolutional layer can be represented as a 3d layer where the 3rd dimension represents a ***set of filters*** that are applied across the layer. A convolutional layer applied multiple filters to its inputs, making it capable of detecting many features in its inputs. 

### CNN Architectures

CNN architectures generally stack a few convolutional layers (each one followed by a ReLU layer), then a pooling layer, then another few convolutional layers, and then another pooling layer and so on. After the repeated pattern is done, a regular feed forward network is added to carry out the rest of the classification.

## Chapter 14: Recurrent Neural Networks

Recurrent Neural Networks are all about telling the future. They can ***analyze time series data*** such as stock prices to tell you when to buy and sell. They can be used in autonomous cars to predict trajectories. More generally, Recurrent Neural Networks work with ***sequences.*** They are also useful in NLP and sentiment analysis. 

Since the output of a recurrent neuron at time step T is a function of all the inputs from previous time steps, you could say it has a form of memory. A part of a neural network that preserves some state across time steps is called a ***memory cell.***

### Input and Output Sequences

A RNN can simultaneously take a sequence of inputs and produce a sequence of outputs. This can be useful in the prediction of time series where you feed it the values over the last N time steps and it must output the value at N + 1. An ***encoder*** is a type of network, also called a sequence-to-vector. You can follow it with a ***decoder.*** An example of using a setup like this in practice is in language translation. A sentence is fed into the encoder to be translated into vector space and then fed through a decoder that knows how to decode into the new language. This works much better than translating on the fly.

## Chapter 15: Autoencoders

***Autoencoders*** are ANNs that are capable of learning efficient representations of the input data, called ***codings*** without any supervision. The codings generally have a much lower dimensionality than the input data which makes them useful for dimensionality reduction. Furthermove, autoencoders act as powerful feature detectors so they can be used to pretrain neural networks. Finally, they are capable of generating new data that looks very similar to the training data. This is called a ***generative model.*** 

Autoencoders work by simply learning to copy their inputs to their outputs. An autoencoder is always composed of two parts: an ***encoder*** that converts the inputs to an internal representation, followed by a ***decoder*** that converts the internal representation to the outputs. In an autoencoder, the number of neurons in the input layer must be equal to the number of neurons in the output. An ***undercomplete*** autoencoder is one that has an internal structure that has a lower dimensionality that the input layers. The undercomplete autoencoder must learn the underlying features to the pattern.

***Stacked Autoencoders*** have multiple hidden layers. Adding layers to the autoencoder allows it to learn more things.

***Variational Autoencoders*** are probabilistic autoencoders, meaning their outputs are partially determined by chance, even after training. More importantly, they are generative autoencoders, meaning they can generate new instances that look like they were sampled form the training set.

## Chapter 16: Reinforcement Learning

I know most of this stuff so I skipped it.

End of book!