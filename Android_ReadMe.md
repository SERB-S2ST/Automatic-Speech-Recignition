Title

Offline Speech Translation Android Application
Vosk ASR + SentencePiece NMT (Native Lib) + Sherpa-ONNX Matcha TTS

1. Overview

This document describes a fully offline Android native application that performs speech-to-speech translation using on-device AI models and native libraries.

The complete pipeline includes:

Automatic Speech Recognition (ASR) using Vosk offline models

Neural Machine Translation (NMT) using SentencePieceProcessor with native C++ inference

Text-to-Speech (TTS) using Sherpa-ONNX Matcha model to convert translated text into audio

The application runs 100% offline, with no cloud APIs, ensuring low latency, privacy, and reliability.

2. End-to-End Architecture

User Speech (Microphone)
        ↓
Vosk ASR Model (Offline)
        ↓
Recognized Source Text
        ↓
SentencePieceProcessor (Native C++)
        ↓
NMT Model (Encoder / Decoder – ONNX / TFLite)
        ↓
Translated Target Text
        ↓
Sherpa-ONNX Matcha TTS
        ↓
PCM Audio Waveform
        ↓
Android AudioTrack Playback

3. Technology Stack

Layer				Technology
Platform			Android (Kotlin)
ASR					Vosk Android SDK
Tokenizer			SentencePieceProcessor (Native C++ JNI)
NMT					ONNX Runtime / TFLite (Native)
TTS					Sherpa-ONNX Matcha
Audio Input			AudioRecord
Audio Output		AudioTrack
Native Layer		Android NDK (C++ / JNI)


4. ASR Module – Vosk

4.1 Description

Vosk performs offline speech recognition, converting microphone audio into source-language text.

4.2 Model

• Example: vosk-model-small-en-us

• Language-specific models supported

4.3 Assets

assets/
 └── vosk/
     └── model/

4.4 Audio Requirements

• Sample Rate: 16 kHz

• Format: PCM 16-bit mono

4.5 Output

• Recognized plain text string

5. NMT Tokenization – SentencePieceProcessor (Native Library)

5.1 Purpose

SentencePieceProcessor is used to:

• Tokenize ASR output into subword units

• Match training-time vocabulary

• Detokenize translated output text

5.2 Native Integration

• Implemented in C++ using SentencePiece

• Exposed to Android via JNI

• Runs fully on-device

5.3 Model File

assets/
 └── spm/
     └── tokenizer.model

5.4 Input / Output

• Input: UTF-8 text

• Output: Token IDs (int[]) or detokenized string

6. Neural Machine Translation (NMT)

6.1 Description

The NMT module translates text from source language → target language completely offline.

6.2 Model Architecture

• Transformer-based Encoder–Decoder

• Uses SentencePiece subword units

6.3 Model Format

• ONNX (recommended for performance)

• or TensorFlow Lite

6.4 Assets

assets/
 └── nmt/
     ├── encoder.onnx
     ├── decoder.onnx
     └── vocab.txt (optional)


6.5 Inference Steps

• SentencePiece tokenization

• Encoder inference

• Decoder inference (greedy / beam search)

• SentencePiece detokenization

6.6 Output

• Translated target-language text

7. Text-to-Speech – Sherpa-ONNX Matcha

7.1 Description

Sherpa-ONNX Matcha converts translated text into natural-sounding speech completely offline.

7.2 Model

• Matcha-TTS (Icefall)

• High-quality neural TTS

7.3 Assets

	assets/
	└── tts/
	├── model.onnx
	├── tokens.txt
	└── lexicon.txt (optional)

7.4 Audio Output

• Sample Rate: 22050 Hz / 24000 Hz

• Output: Float PCM → Int16 PCM

8. Native Layer (NDK + JNI)

8.1 Responsibilities

• SentencePiece tokenization & detokenization

• NMT model inference

• Sherpa-ONNX Matcha inference



8.2 Native Libraries

• libsentencepiece.so

• libonnxruntime.so

• libsherpa-onnx-jni.so

8.3 Supported ABIs

• arm64-v8a (recommended)

• armeabi-v7a (optional)

9. Android Application Layer

9.1 Features

• Speech recording

• ASR live text display

• Translated text preview

• Translated speech playback

9.2 Architecture

• MVVM pattern

• ViewModel + StateFlow

• Jetpack Compose UI

10. Permissions

<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />

11. Performance & Optimization

• Load models once at app startup

• Reuse native inference sessions

• Run inference on background threads

• Prefer arm64 builds for better performance



12. Offline Capability Matrix	

Feature					Offline
Speech Recognition		✅
Tokenization			✅
Translation				✅
Text-to-Speech			✅
Audio Playback			✅
13. Summary

This application demonstrates a complete offline speech translation pipeline on Android by combining:

• Vosk ASR models for speech recognition

• SentencePiece-based NMT with native libraries for translation

• Sherpa-ONNX Matcha TTS for translated speech synthesis

The solution is privacy-first, network-independent.
