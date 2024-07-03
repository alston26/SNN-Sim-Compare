import nengo
from nengo.dists import Uniform
import numpy as np

# Define a dynamic input function
from tensorflow.keras.datasets import mnist

# Load the MNIST dataset
(train_images, train_labels), (_, _) = mnist.load_data()

def mnist_input(t):
    index = int(t) % len(train_images)  # Cycle through the dataset
    return train_images[index].flatten() / 255.0  # Normalize pixel values


# Define the model
model = nengo.Network(label='Spiking MNIST')
with model:
    # Define input layer with dynamic input
    input_layer = nengo.Node(mnist_input)

    # Define the neural populations
    exc_layer = nengo.Ensemble(n_neurons=100, dimensions=784, neuron_type=nengo.LIF())
    inh_layer = nengo.Ensemble(n_neurons=100, dimensions=784, neuron_type=nengo.LIF())

    # Define connections
    nengo.Connection(input_layer, exc_layer, synapse=None)
    nengo.Connection(exc_layer, inh_layer, transform=-1 * np.eye(784))
    nengo.Connection(exc_layer, exc_layer, synapse=0.01)

    # Define probes for recording
    exc_output = nengo.Probe(exc_layer.neurons, 'output')
    exc_voltage = nengo.Probe(exc_layer.neurons, 'voltage')
