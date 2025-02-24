import numpy as np
import tensorflow as tf
from tensorflow.keras.datasets import mnist
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Flatten, Conv2D, MaxPooling2D
from tensorflow.keras.utils import to_categorical

# Load MNIST dataset
(X_train, y_train), (X_test, y_test) = mnist.load_data()

# Normalize pixel values to range [0, 1]
X_train = X_train / 255.0
X_test = X_test / 255.0

# Reshape data for CNN (add channel dimension)
X_train = np.expand_dims(X_train, axis=-1)
X_test = np.expand_dims(X_test, axis=-1)

# One-hot encode labels
y_train = to_categorical(y_train, num_classes=10)
y_test = to_categorical(y_test, num_classes=10)

# Define simple neural network model
simple_model = Sequential([
    Flatten(input_shape=(28, 28)),
    Dense(128, activation='relu'),
    Dense(10, activation='softmax')
])

# Compile model
simple_model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

# Train model
simple_model.fit(X_train, y_train, epochs=5, batch_size=32, validation_split=0.2)

# Define CNN model
cnn_model = Sequential([
    Conv2D(32, kernel_size=(3, 3), activation='relu', input_shape=(28, 28, 1)),
    MaxPooling2D(pool_size=(2, 2)),
    Conv2D(64, kernel_size=(3, 3), activation='relu'),
    MaxPooling2D(pool_size=(2, 2)),
    Flatten(),
    Dense(128, activation='relu'),
    Dense(10, activation='softmax')
])

# Compile model
cnn_model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

# Train model
cnn_model.fit(X_train, y_train, epochs=5, batch_size=32, validation_split=0.2)

# Evaluate simple neural network model
simple_loss, simple_accuracy = simple_model.evaluate(X_test, y_test)
print(f"Simple Neural Network - Test Loss: {simple_loss:.4f}, Test Accuracy: {simple_accuracy:.4f}")

# Evaluate CNN model
cnn_loss, cnn_accuracy = cnn_model.evaluate(X_test, y_test)
print(f"Convolutional Neural Network - Test Loss: {cnn_loss:.4f}, Test Accuracy: {cnn_accuracy:.4f}")
