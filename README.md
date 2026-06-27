# Edge-Based Vehicle Verification & Access Control System 🛡️🚗

<img width="1170" height="831" alt="image" src="https://github.com/user-attachments/assets/a3b8054b-6c02-4a86-92b1-0258d54ef92c" />


## 1. Executive Summary
This repository outlines the system architecture and technical implementation of a fully decentralized, Edge-AI-powered vehicle verification system. Designed to modernize access control for restricted zones and smart cities, the system autonomously identifies vehicles, reads multi-lingual license plates, and triggers physical security barriers in real-time. By shifting the computational load from the cloud directly to the edge, the system achieves sub-200ms latency, ensures operational continuity during network outages, and maximizes data privacy.

---

## 2. The Problem Statement
Traditional Automatic License Plate Recognition (ALPR) systems heavily rely on continuous cloud connectivity. This architectural dependency introduces several critical bottlenecks:
*   **High Latency:** Round-trip data transmission to cloud servers causes delays at security gates, leading to traffic congestion.
*   **Bandwidth Consumption:** Streaming continuous high-definition video feeds to centralized servers overwhelms local networks.
*   **Privacy & Security Risks:** Transmitting unencrypted or raw video streams exposes sensitive positional data.
*   **Network Dependency:** Any internet outage results in a complete failure of the physical security checkpoint.

---

## 3. The Edge-First Solution
To address these limitations, I engineered a **100% Edge-Processed Architecture**. Using constrained but powerful hardware (like NVIDIA Jetson / Raspberry Pi), the system ingests RTSP camera feeds, performs complex Deep Learning inference locally, and only transmits lightweight JSON metadata (plate number, timestamp, confidence score) to the central database for authentication.

### Core Technical Stack:
*   **Hardware Architecture:** NVIDIA Jetson series / High-Performance SBCs, High-Definition RTSP IP Cameras, Relay Modules, Microcontrollers.
*   **Computer Vision & AI:** OpenCV, YOLOv8 (Quantized for Edge), EasyOCR / Tesseract (Fine-tuned for Saudi Arabian / Multi-lingual plates).
*   **Backend & API Integration:** Python, C++, RESTful APIs, SQLite (for offline caching & redundancy).
*   **Hardware Interfacing:** Low-level GPIO pin manipulation for physical barrier control.

---

## 4. Deep-Dive: The AI & Hardware Pipeline

### Phase A: Vision & Localization (YOLOv8)
The system captures live video frames via OpenCV. Instead of processing the entire frame for OCR, an optimized YOLOv8 model instantly detects the vehicle and draws a tight bounding box strictly around the license plate. This dynamic cropping reduces the OCR processing time by over 80%.

### Phase B: Optical Character Recognition (Custom OCR)
Saudi license plates present a unique challenge as they contain both Arabic and English characters alongside numbers. The cropped plate image undergoes local preprocessing (grayscale conversion, adaptive thresholding, and morphological transformations) before being fed into a custom-tuned OCR engine. The engine achieves >95% accuracy under various simulated lighting and weather conditions.

### Phase C: Authentication & Edge-to-Cloud Handshake
The extracted alphanumeric string is packaged into a secure JSON payload and sent to a simulated RESTful verification API. To handle network unreliability, the system utilizes a local SQLite database cache; if the network drops, it verifies against the last known authorized local list.

### Phase D: Physical Hardware Triggering
Upon receiving a `200 OK` (Verified) response from the API, the software executes a C++/Python script to manipulate the Edge device’s GPIO pins. This sends a precise voltage signal to a physical relay module, autonomously opening the security gate with zero human intervention.

---

## 5. System Performance & Impact
*   **Inference Latency:** `< 200 milliseconds` (From vehicle detection to gate trigger).
*   **Bandwidth Reduction:** `~92%` (Eliminated continuous video streaming; only bytes of text are sent).
*   **OCR Accuracy:** `> 95%` in multi-lingual extraction.
*   **Autonomy:** Achieved a complete zero-touch verification process.

---

## 6. Repository Note 🔒
*This repository serves as a technical portfolio and architectural showcase of the system. The proprietary source code, trained model weights, and specific API configurations are kept private to protect intellectual property and security-sensitive implementations.*
