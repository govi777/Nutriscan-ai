# 🥑 NutriScan AI — Smart Nutrition Analyzer

An AI-powered web application that analyzes food photos and delivers instant nutritional insights, personalized recipes, and health tracking — all in one place.

Built with **Streamlit**, **Google Gemini Vision API**, and deployed on **Azure Container Instances** using **Docker**.

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=Streamlit&logoColor=white)
![Gemini AI](https://img.shields.io/badge/Google%20Gemini%20AI-8E75B2?style=for-the-badge&logo=google&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)

---

## 🚀 Live Demo
> Deployed on Azure Container Instances  
> **URL:** `http://nutriscan-ai.<region>.azurecontainer.io:8501`

---

## ✨ Features

### 📸 AI Food Scanner
- Upload or capture a food photo
- Gemini Vision API detects calories, macros (Carbs, Protein, Fat)
- Breaks down health benefits and potential risks
- Interactive macro donut chart via Plotly
- Text-to-speech food summary using gTTS

### 👨‍🍳 AI Chef
- Suggests 3 recipes based on the scanned food
- Diet-aware: adapts to Balanced, Keto, or Vegan preferences

### 📊 Health & Progress Tracker
- Daily calorie intake vs BMR goal tracker
- Hydration tracker (Cup / Bottle logging)
- Burn-it-off calculator: Walk, Run, Bike minutes

### 👤 Personalized Profile
- BMI and BMR calculated using Mifflin-St Jeor formula
- Custom daily calorie goals based on age, gender, weight, height, and activity level

### 💬 AI Nutrition Chatbot
- Context-aware chatbot powered by Gemini
- Answers nutrition, diet, and digestion questions

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Streamlit |
| AI Model | Google Gemini 2.5 Flash / Pro (auto-detects best available) |
| Visualization | Plotly |
| Audio | gTTS (Text-to-Speech) |
| Containerization | Docker |
| Cloud Deployment | Azure Container Instances + Azure Container Registry |

---

## 📁 Project Structure

```
nutriscan-ai/
│
├── main.py              # Main Streamlit application
├── requirements.txt     # Python dependencies
├── Dockerfile           # Docker container configuration
└── README.md
```

---

## ⚙️ Run Locally

### Option 1 — Standard Python
```bash
# Clone the repo
git clone https://github.com/your-username/nutriscan-ai.git
cd nutriscan-ai

# Install dependencies
pip install -r requirements.txt

# Run the app
streamlit run main.py
```

### Option 2 — Docker
```bash
# Build the image
docker build -t nutriscan-ai .

# Run the container
docker run -p 8501:8501 nutriscan-ai
```

Open `http://localhost:8501` in your browser.

---

## ☁️ Azure Deployment

```bash
# Login to Azure
az login

# Create resource group
az group create --name nutriscan-rg --location eastus

# Create Azure Container Registry
az acr create --resource-group nutriscan-rg --name nutriscanaicr --sku Basic
az acr login --name nutriscanaicr

# Build and push Docker image
docker tag nutriscan-ai nutriscanaicr.azurecr.io/nutriscan-ai:v1
docker push nutriscanaicr.azurecr.io/nutriscan-ai:v1

# Deploy to Azure Container Instance
az acr update --name nutriscanaicr --admin-enabled true
az container create --resource-group nutriscan-rg --name nutriscan-app \
  --image nutriscanaicr.azurecr.io/nutriscan-ai:v1 \
  --cpu 1 --memory 1 \
  --registry-login-server nutriscanaicr.azurecr.io \
  --registry-username nutriscanaicr \
  --registry-password $(az acr credential show --name nutriscanaicr --query passwords[0].value -o tsv) \
  --dns-name-label nutriscan-ai --ports 8501

# Get live URL
az container show --resource-group nutriscan-rg --name nutriscan-app --query ipAddress.fqdn -o tsv
```

---

## 🔑 API Key Setup

1. Get a free Gemini API key from: https://aistudio.google.com/app/api-keys
2. Open the app in your browser
3. In the sidebar, paste your API key and click **Link Key**
4. The app auto-detects the best available Gemini model

---

## 📜 License
This project is licensed under the MIT License.

> **Disclaimer:** This is an AI-powered tool for educational and lifestyle purposes only. Always consult a healthcare professional for medical advice.
