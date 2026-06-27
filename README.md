# Edge-Based Vehicle Verification and Access Control System 🛡️🚗

## Overview
This project is a decentralized Edge AI solution engineered to streamline security checkpoints through real-time vehicle verification. By bringing computer vision and data processing directly to the edge, the system minimizes cloud-dependency, significantly reduces latency, and enhances operational privacy.

The system processes live camera feeds to detect approaching vehicles, accurately localizes license plates, and extracts alphanumeric data (Arabic and English). This locally processed data is instantly cross-referenced via RESTful APIs for authentication. Once validated, the system seamlessly interfaces with local GPIO hardware to control security barriers, effectively automating the checkpoint process with an impressive inference latency of under 200 milliseconds per vehicle.

## Key Features
*   **Real-Time ALPR (Automatic License Plate Recognition):** Utilizes an optimized YOLOv8 model combined with a custom-tuned OCR engine for precise multi-lingual plate reading.
*   **Edge Computing Architecture:** All vision and data processing occur directly on the edge hardware, ensuring <200ms latency and maximizing data privacy.
*   **Automated Hardware Control:** Direct GPIO interfacing to manage physical relays and security barriers based on instantaneous API verification results.
*   **Database Integration:** Seamless real-time cross-referencing with central vehicle databases (via RESTful API) and offline SQLite caching for network resilience.
*   **Containerized Deployment:** Packaged using Docker for scalable and robust deployment across various edge environments.

## Tech Stack
*   **Hardware:** NVIDIA Jetson / Raspberry Pi, High-Definition RTSP IP Cameras, Relay Modules.
*   **Languages:** Python, C++.
*   **AI & Computer Vision:** OpenCV, YOLOv8, EasyOCR / Tesseract.
*   **Backend & Interfacing:** RESTful APIs, SQLite, Docker, GPIO manipulation.

## System Workflow
1.  **Detection:** The RTSP camera feed is processed locally; YOLOv8 detects the vehicle and tightly crops the license plate.
2.  **Extraction:** The OCR pipeline extracts Arabic and English characters from the localized plate.
3.  **Verification:** The extracted data is sent as a JSON payload to the verification API to check against authorized records.
4.  **Action:** If authenticated, a signal is transmitted via GPIO pins to trigger the relay, safely opening the physical barrier.
