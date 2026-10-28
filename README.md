# capstone-covert-channels
# HTTP Header Covert Channel Research Prototype

An academic prototype implementing an L7 HTTP/1.1 metadata transmission engine paired with passive Network Detection and Response (NDR) analysis using Zeek NSM and RITA.

Developed as part of the Capstone Project for the **Department of Intelligent Information Systems Security Engineering**.

---

## 📌 Project Overview

This research prototype demonstrates a high-capacity communication channel that encapsulates data within standard, IANA-registered HTTP/1.1 request headers. The implementation adheres strictly to **RFC 9110** (HTTP Semantics) and **RFC 9651** (Structured Field Values for HTTP) to ensure standard web protocol parsers accept the network traffic without generating syntax errors.

To benchmark both transmission stealth and detection visibility, the framework includes out-of-band passive network monitoring using **Zeek** for deep protocol logging and **RITA** for statistical beaconing evaluation.

---

## 🏗️ System Architecture

The architecture consists of three core components:

```text
┌──────────────────────────────┐                ┌──────────────────────────────┐
│   Compromised Workstation    │ ──1. In-Band──>│      Target Web Server       │
│   (Python Sender Client)     │ <──4. Decoy─── │      & Python Receiver       │
└──────────────────────────────┘                └──────────────────────────────┘
               │                                               │
               └───────────────── 2. Mirrored Stream ──────────┘
                                        │
                                        ▼
                        ┌──────────────────────────────┐
                        │    Passive NDR Analytics     │
                        │        (Zeek & RITA)         │
                        └──────────────────────────────┘
```

1. **Client Sender ()**: Slices binary data into chunked bit streams, encodes them into standard HTTP request headers, applies randomized inter-arrival timing jitter (50–300 ms), and balances GET/POST request ratios.
2. **Web Server & Receiver ()**: Serves authentic decoy web content (HTML/JSON) while extracting header values, decoding payload chunks, and reconstructing the target file.
3. **Passive NDR Analytics**: Ingests mirrored network traffic out-of-band to evaluate protocol logging in Zeek (, ) and statistical anomaly detection in RITA (beaconing frequency scores, GET/POST distribution ratios).

---

## 🚀 Key Features

* **High-Capacity Header Transmission**: Encodes payload bytes inside standard request headers (e.g., dual SHA-256 ETag digests yielding 512 bits per POST request).
* **Zero-Disk Control Signaling**: Utilizes RFC 5322 email sub-addressing () in request headers for session signaling without dropping configuration files on disk.
* **NDR Evasion Benchmark**: Interleaves GET (decoy/control) and POST (bulk payload) requests with randomized timing jitter to normalize traffic profiles against statistical beaconing classifiers.
* **Blue-Team Detection Pipeline**: Integrated Zeek logging and RITA analytics framework to evaluate detection thresholds and entropy baseline metrics.

---
