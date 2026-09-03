# 🖼️ Image Caption Generation System

An end-to-end Deep Learning project that automatically generates natural language captions for images using a **CNN-LSTM architecture**.

The system combines **Computer Vision** and **Natural Language Processing (NLP)** to understand an image and generate a descriptive caption word by word. The project includes image and text preprocessing, model training, validation, BLEU evaluation, and caption generation using **Beam Search**.

---

## 📌 Project Overview

Image captioning is a multimodal Artificial Intelligence task that combines:

* **Computer Vision** — understanding visual information in images
* **Natural Language Processing** — generating meaningful textual descriptions

In this project, a **Convolutional Neural Network (CNN)** processes the input image and extracts visual features. These features are combined with a sequence-based **LSTM decoder** to generate captions one word at a time.

The model is trained on the **Flickr8k image caption dataset**, where each image is associated with multiple human-written captions.

### Architecture Overview

```text
                    ┌─────────────────┐
                    │   Input Image   │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │   Custom CNN    │
                    │ Feature Extractor│
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Image Features  │
                    └────────┬────────┘
                             │
                             │
Caption Tokens ──────────────┤
                             ▼
                    ┌─────────────────┐
                    │      LSTM       │
                    │ Caption Decoder │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │  Next Word      │
                    │  Prediction     │
                    └─────────────────┘
```

---

# ✨ Features

* 🖼️ Image preprocessing and resizing
* 📝 Caption text preprocessing
* 🔤 Text tokenization and vocabulary creation
* 📏 Sequence padding for captions
* 🧠 Custom CNN for visual feature extraction
* 🔄 LSTM-based caption generation
* 📊 Image-level train, validation, and test splitting
* 🛡️ Prevention of data leakage between image sets
* 📉 Training and validation loss monitoring
* 🎯 Masked accuracy evaluation
* 💾 Best model checkpoint saving
* ⏳ Early stopping to reduce overfitting
* 📉 Learning rate reduction using `ReduceLROnPlateau`
* 📊 BLEU-1, BLEU-2, BLEU-3, and BLEU-4 evaluation
* 🔎 Beam Search for improved caption generation
* 🖼️ Caption generation for unseen images

---

# 🧠 How It Works

The complete image captioning pipeline consists of the following stages:

## 1. Dataset Preparation

The project uses an image-caption dataset where each image is associated with multiple human-written captions.

The dataset is split at the **image level** into:

* **Training Set:** 80%
* **Validation Set:** 10%
* **Test Set:** 10%

Splitting at the image level is important because each image has multiple captions. This prevents the same image from appearing in both training and testing datasets, helping avoid **data leakage**.

---

## 2. Caption Preprocessing

The captions are prepared before training.

The preprocessing pipeline includes:

* Converting captions to lowercase
* Adding `startseq` and `endseq` tokens
* Tokenizing words into numerical IDs
* Creating a vocabulary
* Padding sequences to a fixed length

Example:

```text
Original Caption:

A dog is running on the grass

Processed Caption:

startseq a dog is running on the grass endseq
```

---

## 3. Image Preprocessing

Images are:

* Loaded from the dataset
* Resized to **224 × 224**
* Converted into numerical arrays
* Processed in batches for training

Example image batch shape:

```text
(32, 224, 224, 3)
```

Where:

* `32` = batch size
* `224 × 224` = image dimensions
* `3` = RGB color channels

---

## 4. CNN Feature Extraction

A custom Convolutional Neural Network (CNN) extracts meaningful visual patterns from images.

The CNN learns hierarchical visual features such as:

```text
Edges
   ↓
Textures
   ↓
Shapes
   ↓
Object-related Features
```

The extracted visual representation is then passed to the caption generation model.

---

## 5. LSTM Caption Generation

The LSTM processes the caption as a sequence and predicts the next word.

For example:

```text
Input:

startseq

Target:

a
```

Then:

```text
Input:

startseq a

Target:

dog
```

Then:

```text
Input:

startseq a dog

Target:

is
```

The model continues predicting words until the `endseq` token is generated.

---

# 🔎 Beam Search Caption Generation

Instead of using only greedy decoding, this project uses **Beam Search** during caption generation.

Greedy decoding selects only the most probable word at every step.

```text
startseq
    ↓
    a
    ↓
   dog
    ↓
    is
```

Beam Search keeps multiple possible caption sequences and evaluates them during generation.

```text
                 ┌── a man is ...
startseq ── a ───┤
                 ├── a woman is ...
                 │
                 └── a child is ...
```

This can help generate more coherent captions by considering multiple possible sequences.

---

# 📊 Dataset

The project uses the **Flickr8k Dataset**.

The processed dataset contains approximately:

* **8,091 images**
* **40,453 captions**

Dataset split:

| Dataset    | Images | Captions |
| ---------- | -----: | -------: |
| Training   |  6,472 |   32,358 |
| Validation |    809 |    4,045 |
| Test       |    810 |    4,050 |

> Note: Dataset files are not included in this repository due to their size. Please download the Flickr8k dataset separately and configure the dataset paths in the notebook.

---

# 📈 Model Training

The model training pipeline includes several techniques to improve generalization and training stability.

### Model Checkpoint

The best-performing model is saved based on validation performance.

### Early Stopping

Training automatically stops when validation performance no longer improves.

### Reduce Learning Rate on Plateau

The learning rate is reduced when the validation loss stops improving, allowing the model to make smaller updates during later training stages.

---

# 📊 Evaluation

The model is evaluated using:

* Test Loss
* Masked Accuracy
* BLEU-1
* BLEU-2
* BLEU-3
* BLEU-4

### Why BLEU?

Traditional word-level accuracy does not fully represent caption quality.

For example:

```text
Reference:

A dog is running on the grass

Generated:

A brown dog is playing on the grass
```

The generated caption may still describe the image correctly even though some words differ.

BLEU evaluates the similarity between generated captions and human-written reference captions.

---

# 📊 Results

The trained model achieved the following evaluation results:

| Metric               |      Score |
| -------------------- | ---------: |
| Test Loss            | **2.7774** |
| Test Masked Accuracy | **39.38%** |
| BLEU-1               | **0.4974** |
| BLEU-2               | **0.3207** |
| BLEU-3               | **0.2059** |
| BLEU-4               | **0.1374** |

The BLEU scores naturally decrease as the n-gram length increases because matching longer word sequences exactly is more difficult.

```text
BLEU-1 > BLEU-2 > BLEU-3 > BLEU-4
```

These results provide a baseline for evaluating the CNN-LSTM image captioning approach.

---

# 🛠️ Technology Stack

### Programming Language

* Python

### Deep Learning

* TensorFlow
* Keras

### Data Processing

* NumPy
* Pandas

### Visualization

* Matplotlib

### NLP and Evaluation

* NLTK

---

# 📂 Project Structure

```text
Image-Caption-Generation-system/
│
├── ImageCaptionGeneration.ipynb
│
├── README.md
│
├── requirements.txt
│
└── models/                  # Generated after training
    └── best_caption_model.keras
```

> The exact folder structure may vary depending on where the dataset and trained models are stored.

---

# 🚀 How to Run

## 1. Clone the Repository

```bash
git clone https://github.com/Dimuthu21/Image-Caption-Generation-system.git
```

Move into the project directory:

```bash
cd Image-Caption-Generation-system
```

---

## 2. Create a Virtual Environment (Recommended)

### Windows

```bash
python -m venv venv
```

Activate it:

```bash
venv\Scripts\activate
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 4. Download the Dataset

Download the Flickr8k dataset and place the images and caption files in the appropriate directories.

Update the dataset paths in the notebook if necessary.

---

## 5. Run the Notebook

Start Jupyter Notebook:

```bash
jupyter notebook
```

Then open:

```text
ImageCaptionGeneration.ipynb
```

Alternatively, the notebook can be run using **Google Colab**.

---

# 🖼️ Generating Captions

After training the model, provide an unseen image to the caption generation pipeline.

Example:

```text
Input Image
      ↓
CNN extracts visual features
      ↓
LSTM generates caption tokens
      ↓
Beam Search evaluates candidate sequences
      ↓
Generated Caption
```

Example output:

```text
Input:
🖼️ Image of a dog running outdoors

Generated Caption:
"A dog is running through the grass"
```

> Generated captions may vary depending on the image and trained model.

---

# ⚠️ Current Limitations

Although the system successfully implements the complete image captioning pipeline, there are several areas for future improvement.

### Custom CNN Training

The CNN is trained from scratch. A pretrained model could potentially provide stronger visual features.

### Caption Repetition

Sequence generation models can sometimes produce repetitive or generic captions.

### Dataset Limitations

The model is limited by the diversity and size of the Flickr8k dataset.

### LSTM Limitations

LSTMs can struggle with long-range dependencies compared with modern Transformer architectures.

---

# 🚀 Future Improvements

Potential improvements include:

* 🔹 Using pretrained CNN backbones such as EfficientNet or ResNet
* 🔹 Applying transfer learning
* 🔹 Experimenting with Transformer-based caption decoders
* 🔹 Using Vision Transformers for image representations
* 🔹 Improving caption decoding strategies
* 🔹 Increasing dataset diversity
* 🔹 Adding attention mechanisms
* 🔹 Comparing CNN-LSTM models with modern vision-language architectures
* 🔹 Deploying the trained model as a web application using Streamlit or FastAPI

---

# 🎓 Key Learning Outcomes

This project provided practical experience in:

* Deep Learning
* Computer Vision
* Natural Language Processing
* CNN architectures
* LSTM sequence modelling
* Image and text preprocessing
* Dataset splitting and data leakage prevention
* Model training and validation
* Early stopping and checkpointing
* Learning rate scheduling
* BLEU evaluation
* Beam Search decoding

---


# 📄 License

This project is created for educational and portfolio purposes.

If you find this project useful, feel free to ⭐ star the repository!
