# keras-sharp

[![Join the chat at https://gitter.im/keras-sharp/Lobby](https://badges.gitter.im/keras-sharp/Lobby.svg)](https://gitter.im/keras-sharp/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
An ongoing effort to port most of the Keras deep learning library to C#

Welcome to the Keras# project! We aim to bring a experience-compatible Keras-like API to C#, meaning that, if you already know Keras, you should not have to learn any new concepts to get up and running with Keras#. This is a direct port, line-by-line port of the Keras project, meaning that applying updates and fixes commited to the main Keras project should be simple and straightforward. As in the original project, we aim to support both TensorFlow and CNTK (but not Theano, as it [has been recently discontinued](https://groups.google.com/d/msg/theano-users/7Poq8BZutbY/rNCIfvAEAwAJ)).

## Example

Consider the following [Keras Python example originally done by Jason Brownlee](https://machinelearningmastery.com/tutorial-first-neural-network-python-keras/), reproduced below:

```python
from keras.models import Sequential
from keras.layers import Dense
import numpy

# fix random seed for reproducibility
numpy.random.seed(7)

# load pima indians dataset
dataset = numpy.loadtxt("pima-indians-diabetes.csv", delimiter=",")
# split into input (X) and output (Y) variables
X = dataset[:,0:8]
Y = dataset[:,8]

# create model
model = Sequential()
model.add(Dense(12, input_dim=8, activation='relu'))
model.add(Dense(8, activation='relu'))
model.add(Dense(1, activation='sigmoid'))

# Compile model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

# Fit the model
model.fit(X, Y, epochs=150, batch_size=10)

# evaluate the model
scores = model.evaluate(X, Y)
print("\n%s: %.2f%%" % (model.metrics_names[1], scores[1]*100))
```

The same can be obtained using Keras# using:

```csharp
// Load the Pima Indians Data Set
var pima = new Accord.DataSets.PimaIndiansDiabetes();
float[,] x = pima.Instances.ToMatrix().ToSingle();
float[] y = pima.ClassLabels.ToSingle();

// Create the model
var model = new Sequential();
model.Add(new Dense(12, input_dim: 8, activation: new ReLU()));
model.Add(new Dense(8, activation: new ReLU()));
model.Add(new Dense(1, activation: new Sigmoid()));

// Compile the model (for the moment, only the mean square 
// error loss is supported, but this should be solved soon)
model.Compile(loss: new MeanSquareError(), 
    optimizer: new Adam(), 
    metrics: new[] { new Accuracy() });

// Fit the model for 150 epochs
model.fit(x, y, epochs: 150, batch_size: 10);

// Use the model to make predictions
float[] pred = model.predict(x)[0].To<float[]>();

// Evaluate the model
double[] scores = model.evaluate(x, y);
Console.WriteLine($"{model.metrics_names[1]}: {scores[1] * 100}");
```

Upon execution, you should see the same familiar Keras behavior as shown below:

![Keras Sharp during training](https://github.com/cesarsouza/keras-sharp/raw/master/Docs/Wiki/learning.png)


## Work-in-progress

However, please note that this is still work-in-progress. Not only Keras#, but also [TensorFlowSharp](https://github.com/migueldeicaza/TensorFlowSharp) and [CNTK](https://github.com/Microsoft/CNTK). If you would like to contribute to the development of this project, please consider submitting new issues to any of those projects, [including us](https://github.com/cesarsouza/keras-sharp/issues).
