# GenAI Physics Simulator for SimPhy

[cite_start]This project is a web-based application that uses a Retrieval-Augmented Generation (RAG) AI model to translate natural language prompts into executable JavaScript code for the SimPhy physics simulation software. [cite: 1] [cite_start]Users can describe a complex physics setup in plain English, and the AI will generate the corresponding, accurate simulation script. [cite: 1]

## ✨ Key Features

* [cite_start]**Natural Language to Code:** Translates high-level descriptions (e.g., "a pendulum on a moving cart") into precise, runnable code. [cite: 1]
* [cite_start]**RAG-Powered Accuracy:** Uses a custom knowledge base built from the SimPhy source code, ensuring the AI uses real, valid API functions instead of guessing. [cite: 1]
* [cite_start]**Complex Logic Generation:** Capable of understanding and creating scripts for multi-body systems with various joints, forces, and relationships. [cite: 1]
* [cite_start]**Automated Workflow:** Generates a downloadable `.js` file, eliminating the need for manual copy-pasting and reducing errors. [cite: 1]
* [cite_start]**Simple & Fast Interface:** A clean, single-page web app for quick and easy interaction. [cite: 1]
* **(NEW) Goal-Oriented Optimization:** Define a goal, and the system runs hundreds of experiments to find the optimal parameters.
* **(NEW) Multi-Simulation Analysis:** Intelligently explores a range of variables to discover the best possible outcome.
* **(NEW) Live Interactive Dashboard:** Visualizes the results of all experiments in real-time on a 3D scatter plot and presents live statistics.

## 🏛️ Architecture

### Original RAG Architecture
[cite_start]The application uses a modern RAG architecture to achieve high accuracy with a proprietary, undocumented API. [cite: 1]

1.  **Knowledge Base:** A `knowledge_base.json` file was created by analyzing SimPhy's Java source code. [cite_start]This file acts as a detailed "textbook" of the scripting API. [cite: 1]
2.  [cite_start]**User Prompt:** The user provides a prompt via the web interface. [cite: 1]
3.  [cite_start]**RAG Pipeline (Backend):** The Node.js server receives the prompt, augments it with the entire knowledge base, and sends this rich context to the OpenAI API. [cite: 1]
4.  [cite_start]**Informed Generation:** The AI model uses the provided rules, examples, and API definitions to generate a logically sound and syntactically correct script. [cite: 1]
5.  [cite_start]**Code to User:** The generated script is sent back to the frontend to be displayed and downloaded. [cite: 1]

### (NEW) Optimization Engine Architecture
The new optimization feature is a multi-part, event-driven system built around the original AI server.

1.  **The Runner (Python):** Orchestrates the analysis by generating hundreds of prompts and calling the original AI server to get the corresponding scripts. It then runs these scripts headlessly.
2.  **Data Pipeline (Kafka):** A real-time message bus that transports raw simulation results from the Runner to the Analyzer.
3.  **Pathway Intelligence Core (Python):** The brain of the operation. It ingests the raw data stream from Kafka, performs real-time analysis, and publishes insights to new Kafka topics.
4.  **Dashboard Backend (Python/Flask):** A web server that serves the dashboard and streams the live insights from Pathway to the user's browser using WebSockets.

## 📁 File Structure

This project consists of several key files working together:

### Original Files
* [cite_start]`server.js`: The Node.js backend server for the RAG AI model. [cite: 1]
* [cite_start]`index.html`: The complete single-page frontend for the original application. [cite: 1]
* [cite_start]`knowledge_base.json`: The "brain" of our RAG system. [cite: 1]
* [cite_start]`package.json` & `package-lock.json`: Standard Node.js files that manage the project's dependencies. [cite: 1]
* [cite_start]`.env`: A local, private file used to store secret keys. [cite: 1]

### (NEW) Optimization Engine Files
* `runner.py`: The new script that orchestrates the multi-simulation analysis.
* `analyzer.py`: The new Pathway script that performs real-time analysis.
* `dashboard.py`: The new Flask server for the live dashboard.
* `templates/dashboard.html`: The new frontend for the live dashboard.
* `docker-compose.yml`: Configuration file to easily run Kafka.

## 🚀 Setup and Installation

### Prerequisites
* Node.js
* Python 3.8+
* Docker Desktop

### Installation Steps

1.  **Clone the repository:**
    ```bash
    git clone <your-repo-url>
    cd <your-repo-folder>
    ```

2.  **Install Node.js dependencies:**
    ```bash
    npm install
    ```

3.  **Install Python dependencies:**
    ```bash
    pip install pathway kafka-python requests flask flask-socketio
    ```

4.  **Create the environment file:**
    * [cite_start]Create a new file named `.env` in the root of the project. [cite: 1]
    * [cite_start]Add your API key to it: [cite: 1]
        ```
        OPENAI_API_KEY="YOUR_API_KEY_HERE"
        ```

## ⚡ How to Run

### Running the Original Script Generator
1.  Start the server:
    ```bash
    node server.js
    ```
2.  [cite_start]Open your web browser and navigate to `http://localhost:3000`. [cite: 1]

### (NEW) Running the Full Optimization Engine
You will need to run four separate services in four separate terminals.

1.  **Terminal 1: Start Kafka**
    ```bash
    docker-compose up
    ```

2.  **Terminal 2: Start the AI Script Generator**
    ```bash
    node server.js
    ```

3.  **Terminal 3: Start the Pathway Analyzer**
    ```bash
    python analyzer.py
    ```

4.  **Terminal 4: Start the Dashboard Server**
    ```bash
    python dashboard.py
    ```
5.  **Open the Dashboard:** Once all services are running, navigate to `http://localhost:5000`.

## 🧑‍💻 How to Use

### Original App
1.  [cite_start]**Enter a Prompt:** Type a description of the 2D mechanics simulation you want to create in the text box. [cite: 1]
2.  [cite_start]**Generate Script:** Click the "Generate Script" button. [cite: 1]
3.  [cite_start]**Review & Download:** The generated JavaScript code will appear in the display box. [cite: 1]

### (NEW) Optimization Engine
1.  **Open the Dashboard:** Navigate to `http://localhost:5000`.
2.  **Start Analysis:** Click the "Start Analysis" button to begin the optimization process with the default parameters.
3.  **Watch Live:** Observe as the dashboard populates with data in real-time. The 3D plot will show each simulation run, and the stat cards will update with the latest insights and the current best solution found.
