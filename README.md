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
