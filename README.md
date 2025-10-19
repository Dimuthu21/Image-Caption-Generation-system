🖼️ Image Caption Generation System
📌 Project Overview

This project is an Image Caption Generation System built using TensorFlow and Keras in Google Colab. It automatically generates descriptive captions for images by combining CNNs for visual feature extraction and RNNs with attention for natural language generation.

⚙️ Features

Preprocessing of images and captions

CNN encoder + RNN decoder with attention

Model training with caption datasets

Caption prediction for unseen images

Evaluation with NLP metrics

🛠️ Tech Stack

Python

TensorFlow / Keras

NumPy, Pandas, Matplotlib

Keras Tuner

📂 Dataset

Images are passed through a CNN to extract features.

Captions are tokenized, padded, and mapped to vocabulary indices.

🚀 How to Run

Clone the repo:

git clone https://github.com/your-username/Image-Caption-Generation-System.git
cd Image-Caption-Generation-System


Install dependencies:

pip install -r requirements.txt


Open the notebook in Jupyter/Colab:

jupyter notebook ImageCaptionGeneration.ipynb

📦 Requirements

Listed in requirements.txt:

tensorflow
keras
keras-tuner
numpy
pandas
matplotlib
pickle-mixin

📊 Results

Generates meaningful captions for unseen images.

Example:

Input Image → 🖼️

Generated Caption → “A group of people standing near a beach”
