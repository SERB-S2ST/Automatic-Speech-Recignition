# README for ASR


# 🎤 Real-Time Speech Recognition using VOSK (Dockerized)

This project implements a **real-time microphone-based Automatic Speech Recognition (ASR)** system using the **VOSK** speech recognition engine. The entire setup is **Dockerized** for easy deployment, portability, and reproducibility.

---

## 🚀 Features

- Real-time speech recognition from microphone  
- Offline ASR using VOSK  
- Fully Dockerized setup  
- Direct access to system microphone (`/dev/snd`)  
- Fast and lightweight inference  
- Easily portable across Linux systems

Training a TDNN Model in Kaldi
Kaldi trains neural network acoustic models through several structured stages. A TDNN
(Time-Delay Neural Network) is commonly trained using the chain (LF-MMI) framework, which
provides strong performance for modern ASR systems.
1. Data Preparation
Organize your speech data into Kaldi’s standard format. Each dataset (train, dev, test) must
include audio paths, speaker information, and the reference transcripts. At this stage, prepare a
pronunciation lexicon containing words and their phonetic transcriptions, along with lists of
silence and non-silence phones.
2. Feature Extraction
Kaldi extracts features such as MFCCs and computes normalization statistics. These features
form the acoustic representation used during both GMM and neural network training.
3. GMM Training for Alignments
Even though the final model is neural, Kaldi requires initial GMM-HMM models to obtain
high-quality alignments between audio frames and phonetic labels.
Train a sequence of increasingly accurate GMM models: monophone, triphone, LDA+MLLT,
and SAT. The final SAT system generates alignments that TDNN training depends on.
4. Preparing Chain Training Inputs
Chain training uses a special topology and additional supervision. After GMM alignment, Kaldi
prepares a modified language directory and generates training lattices. These provide richer
supervision for LF-MMI training.
5. i-Vector Extraction
To make the neural network more robust to speaker variability, Kaldi extracts i-vectors for the
training data. These speaker embeddings are provided as auxiliary inputs during TDNN training.
6. TDNN (Chain) Training
The TDNN architecture models temporal context by connecting each layer to specific time
offsets instead of all frames.
Kaldi's chain training script optimizes the model using LF-MMI, which directly maximizes
sequence-level performance.
During training, it uses:
● Frame features (MFCCs)
● i-vectors
● Supervisory lattices
● Chain topology
The output is a fully trained TDNN acoustic model.
7. Decoding
Once trained, the TDNN model is combined with a decoding graph that includes the lexicon,
language model, and HMM topology. Using this graph, the system recognizes speech from test
audio.
Building an N-gram Language Model
An N-gram language model estimates word sequence probabilities based on counts in a text
corpus.
1. Collect LM Text
You gather a large text file containing sentences in your target language domain. This will serve
as the statistical foundation for the LM.
2. Train the N-gram Model
A tool like IRSTLM, SRILM or KenLM reads the text and produces an N-gram
model—commonly 3-gram or 4-gram.
Smoothing methods (like Kneser-Ney) help estimate probabilities for unseen or rare
sequences.
3. Convert LM to Kaldi Format
The N-gram LM is stored in ARPA format. Kaldi converts this into its finite-state transducer
(FST) form and integrates it with the lexicon.
This results in a decoding language directory containing the grammar graph used during
recognition.
4. Use LM in Decoding
The TDNN acoustic model is combined with the N-gram LM to produce a complete ASR
decoder. The LM helps choose the most linguistically plausible word sequence during
recognition.

---

## 📁 Project Structure

```text
.
├── serb_cnn_v1_model/
│   └── serb_cnn_model/
├── utils_all.py
├── test_microphone.py
├── Dockerfile
└── README.md

---

## BUILD THE DOCKER IMAGE

---

Go to project folder and then open the terminal and run below command : 



sudo docker build -t vosk_speech_asr .
	

---

## RUN THE CONTAINER 

---

sudo docker run -it --rm --device /dev/snd vosk_speech_asr

NOTE : The run command requires access to /dev/snd on the host system for audio input.

---
