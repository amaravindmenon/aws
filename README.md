# ☁️ LearnLocalAWS

[![React](https://img.shields.io/badge/Frontend-React%20%2B%20Vite-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev/)
[![Express](https://img.shields.io/badge/Backend-Node.js%20%2B%20Express-339933?style=for-the-badge&logo=node.js&logoColor=white)](https://expressjs.com/)
[![Moto](https://img.shields.io/badge/Emulator-Python%20Moto-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/getmoto/moto)
[![License](https://img.shields.io/badge/License-MIT-brightgreen?style=for-the-badge)](LICENSE)

**LearnLocalAWS** is a premium, fully local AWS learning platform and emulator dashboard. Built with a beautiful, modern **AWS-dark themed GUI**, it lets you develop, test, and master core AWS services entirely on your local machine—with **zero risk of cloud bills, no Docker overhead, and instant feedback loops**.

Whether you're practicing S3 bucket policies, writing and testing Lambda functions in an in-browser code editor, simulated time-warping files into Glacier, or modeling serverless ECS Fargate task allocations, LearnLocalAWS brings the cloud straight to your local environment.

---

## 🏗️ Architecture

```
┌────────────────────────────────┐       ┌────────────────────────────────┐       ┌────────────────────────────────┐
│       React + Vite UI          │ ───►  │     Express API Gateway        │ ───►  │      Python Moto Emulator      │
│      (Local Port: 5173)        │ ◄───  │      (Local Port: 3001)        │ ◄───  │       (Local Port: 5100)       │
└────────────────────────────────┘       └────────────────────────────────┘       └────────────────────────────────┘
                                                         │
                                                         ▼
                                         ┌────────────────────────────────┐
                                         │       Node JS VM Sandbox       │
                                         │       (Lambda Executor)        │
                                         └────────────────────────────────┘
```

- **Frontend (React + Vite)**: A state-of-the-art console interface leveraging modern typography, dynamic animations, customized glassmorphic charts, and dark-mode aesthetics resembling the real AWS Console.
- **Backend (Node.js + Express)**: Handles API requests, performs VM-level orchestration, coordinates serverless Lambda deployments via a lightweight JavaScript `vm` sandbox, and pipes raw REST commands directly into the emulator.
- **Emulator (Python Moto Server)**: Standard AWS mockup core serving robust local S3 APIs without massive Docker container footprints.

---

## 🚀 Key Features

### 📦 Simple Storage Service (S3)
*   **Complete Lifecycle Actions**: Create, list, open, empty, and delete buckets.
*   **Object File Hub**: Upload via click & drag-and-drop, download objects, delete objects, and view files.
*   **Time Warp Lifecycle Simulator**: Instantly warp time forward to see standard objects transition to Glacier classes based on lifecycle rules.
*   **S3 Bucket-Level & Object-Level Tagging**: Attach, edit, and read key-value tags at the bucket level (for billing models) or directly onto individual objects.
*   **Custom Metadata Hub**: Modify and persist custom object headers dynamically (e.g. `x-amz-meta-`).
*   **Pre-signed URLs**: Generate ready-to-use temporary download links instantly.
*   **Bucket Policy Simulator**: Write JSON bucket access policies and verify permissions in a local mock tester.
*   **S3 Versioning**: Enable versioning and inspect delete markers, previous object states, and rollback options.

### ⚡ Serverless Compute (Lambda)
*   **In-Browser Code Editor**: A built-in code editor to write, modify, and format your local Lambda functions.
*   **VM Sandbox Executor**: Trigger handlers securely with zero Docker start lag. Supports asynchronous handlers, node core standard imports, and JSON event testing payloads.
*   **Real-time Console Capture**: Captures all standard `console.log()` streams directly in the browser's execution tab.
*   **Variables & Config**: Custom environment variable stores, customizable memory configurations, and execution timeout adjusters.
*   **Local Persistent Store**: Functions are persisted locally in `backend/data/functions.json` for reliable persistence across restarts.

### 🏢 Container & Orchestration Mocks
*   **ECS & Serverless Fargate**: Allocates task counts, spins up mock tasks, tracks visual provisioning steps, and streams virtual live terminal stdout/stderr logs.
*   **EKS Replica Scheduler**: Controls replica sets, deploys pod replicas, simulates node schedulers, and previews proxy load balancer entry points.

### 💸 Core Dashboard & Financials
*   **Cost Explorer upgraded simulator**: Dynamic billing projections, interactive HSL customized charts, service budget caps, and simulated resource footprint alerts.
*   **AWS Services Mocks**: Local routes configured for **DynamoDB**, **IAM**, **KMS**, **Route53**, **RDS & Aurora**, **Secrets Manager**, **SNS**, **SQS**, **Lightsail**, and **API Gateway Stage deployments**.
*   **Global Custom Dialogs**: Complete standard modal overrides (`ConfirmModal` & `SuccessModal`) replaces boring native browser alerts with premium, non-blocking UI.

---

## 🛠️ Installation & Setup

Follow these steps to set up LearnLocalAWS on your local machine:

### Prerequisites
*   **Node.js** (v18.x or higher recommended)
*   **Python** (v3.8 or higher recommended)
*   **npm** (bundled with Node.js)

### Step 1: Clone the Repository & Configure Env
1. Clone this repository to your target folder:
   ```bash
   git clone <your-repo-url> LearnLocalAWS
   cd LearnLocalAWS
   ```
2. A pre-configured `.env` is located at the root directory. Feel free to modify the ports if needed:
   ```env
   # === Emulator (Moto Python Server) ===
   EMULATOR_HOST=http://127.0.0.1
   EMULATOR_PORT=5100

   # === Backend API Server ===
   BACKEND_PORT=3001

   # === Frontend Dev Server ===
   FRONTEND_PORT=5173

   # === Fake AWS Credentials (used locally) ===
   AWS_ACCESS_KEY_ID=test
   AWS_SECRET_ACCESS_KEY=test
   AWS_REGION=us-east-1
   ```

### Step 2: Install Node Dependencies
Use the root installer to automatically set up dependencies for both the `backend` and `frontend` folders:
```bash
npm install
npm run install:all
```

### Step 3: Install Python Emulator Dependencies
Install the required packages for the Python Moto server. It is recommended to use a virtual environment:
```bash
# Optional: Create and activate a virtual environment
python -m venv venv
# On Windows (CMD):
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install requirements
pip install -r emulator/requirements.txt
```

---

## 🏃 Running the Application

You can start all three services simultaneously or launch them individually.

### Option A: The Easy Way (One Terminal)
From the root directory, simply run:
```bash
npm start
```
This triggers the `concurrently` package to boot up the **Emulator**, **Express Backend**, and **Vite Frontend** in parallel.

### Option B: The Individual Way (Three Terminals)
For debugging or reading logs separately, run each script in a separate terminal window:

1.  **Terminal 1 — AWS Emulator**
    ```bash
    python emulator/server.py
    ```
2.  **Terminal 2 — Backend API**
    ```bash
    cd backend
    node src/index.js
    ```
3.  **Terminal 3 — Frontend Console**
    ```bash
    cd frontend
    npm run dev
    ```

Once all servers are running, navigate to:
👉 **[http://localhost:5173](http://localhost:5173)**

---

## ⚙️ Key Technical Decisions

1.  **Raw HTTP Direct REST Calls for S3**:
    The official AWS SDK v3 (`@aws-sdk/client-s3`) appends custom checksum headers (`x-amz-checksum-*`) that cause compatibility issues in older Moto packages. LearnLocalAWS utilizes pure REST calls using Node's `fetch`/`axios` wrapper which ensures 100% stable communication with the local emulator.
2.  **Node.js `vm` Sandboxing**:
    Instead of spinning up heavy local Docker containers for Lambda invocations, LearnLocalAWS executes Javascript handler functions inside Node's native `vm` module sandbox. This guarantees near-zero millisecond startup times and fully captures console streams safely without resource bloat.
3.  **Local Persistent JSON Store**:
    Standard Python Moto mock servers clear their memory buffers on restart. To deliver an outstanding developer experience, Lambda metadata, tags, and settings are written directly to a local JSON file database (`backend/data/functions.json`), ensuring your work remains safe.

---

## 📂 Project Directory Structure

```
LearnLocalAWS/
├── .env                        # Port and credential overrides
├── package.json                # Main runner and project setup scripts
├── walkthrough.md              # Feature status logs and screenshot indices
├── emulator/
│   ├── server.py               # Flask-wrapped Moto server
│   └── requirements.txt        # Moto, Flask, and CORS dependencies
├── backend/
│   ├── package.json
│   └── src/
│       ├── index.js            # Express Entrypoint
│       ├── config.js           # Env parser
│       ├── routes/             # REST routing layers
│       │   ├── s3.js           # S3 API routing
│       │   ├── lambda.js       # Lambda configuration routing
│       │   ├── dynamo.js       # DynamoDB mock controller
│       │   ├── cost.js         # Cost Explorer dashboard router
│       │   └── ... (20+ AWS service mocks)
│       └── services/           # Service orchestration core
│           ├── s3Service.js    # Raw REST integration wrapper
│           └── lambdaService.js# Sandbox execution & local database
└── frontend/
    ├── vite.config.js          # Vite config
    └── src/
        ├── App.jsx             # Top-level Routing & App Frame
        ├── index.css           # Global AWS-dark CSS design tokens
        ├── components/         # Reusable AWS layout components
        │   ├── Header.jsx      # Navigation bar
        │   ├── Sidebar.jsx     # Service selection panel
        │   └── Settings.jsx    # Portal emulator connection editor
        └── pages/              # GUI view components
            ├── Dashboard.jsx   # Cost Explorer and billing overview
            ├── S3/             # S3 GUI components
            └── Lambda/         # Lambda IDE & logs UI panels
```

---

## 📝 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*Made with ❤️ for offline AWS developers. Build fast, pay $0.00!*
