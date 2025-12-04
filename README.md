# 💸 Local Model AI Expense — n8n + Google Drive + Sheets

![License](https://img.shields.io/github/license/matiasvallejosdev/ai-expense-tracker-n8n?label=license&logo=github&color=f80&logoColor=fff)
![Forks](https://img.shields.io/github/forks/matiasvallejosdev/ai-expense-tracker-n8n.svg)
![Stars](https://img.shields.io/github/stars/matiasvallejosdev/ai-expense-tracker-n8n.svg)
![Watchers](https://img.shields.io/github/watchers/matiasvallejosdev/ai-expense-tracker-n8n.svg)

> Automatically extract and organize expenses from bank statements (PDFs/images) into Google Sheets using AI, n8n, and a modern web interface

![Workflow Overview](docs/workflow.jpg)

## 📘 Introduction

**AI Expense Tracker** is a low-code automation built with **n8n** that leverages **OpenAI**, **Google Drive**, and **Google Sheets** to automate personal finance tracking.

**Two ways to use it:**

- 🌐 **Web App** – Upload receipts and bank statements (PDFs or images) via a simple Next.js interface (Soon, a dashboard)
- 📁 **Google Drive** – Drop files into a folder and let automation handle the rest

Just upload your **bank statement PDF or receipt image**, and the workflow will:

1. Extract the data using AI (supports PDFs and images).
2. Categorize each expense intelligently.
3. Append the results to a structured Google Sheet dashboard.

Inspired by *"El Hombre Más Rico de Babilonia"* and *"La Bolsa o la Vida"*, this project promotes financial awareness and digital independence through automation.

## ✨ Key Features

- **🐳 Docker Support** – One-command setup with Docker Compose. Works on any machine.
- **🌐 Web App Interface** – Upload receipts and bank statements via a simple web UI (supports PDFs and images).
- **Google Drive Integration** – Drop your bank statement PDFs into a folder, and the system takes it from there.
- **AI-Powered Parsing** – Uses GPT-4o to read and structure your expenses into JSON.
- **Categorization Agent** – Classifies expenses into fixed categories (Supermarket, Gastronomy, etc.).
- **Automatic Google Sheets Sync** – Generates a new "Expenses {Month}" tab and appends all transactions.
- **Open Source** – Fully local, no external servers, and zero maintenance cost.

## 🚀 Quick Start (Docker - Recommended)

The easiest way to get started is with Docker Compose. This will set up both the n8n automation engine and the web app in one command.

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed.
- A [Google Cloud Project](https://console.cloud.google.com/) with:
  - **Google Drive API** enabled.
  - **Google Sheets API** enabled.
- A Google Sheet based on the provided template (`/templates/balance.xlsx`).

### Steps

1. **Clone the repository**

   ```bash
   git clone https://github.com/matiasvallejosdev/ai-expense-tracker-n8n.git
   cd ai-expense-tracker-n8n
   ```

2. **Configure environment variables**

   ```bash
   cp .env.example .env
   ```

   Edit `.env` and add your Google Sheets dashboard URL:

   ```bash
   NEXT_PUBLIC_DASHBOARD_URL=https://docs.google.com/spreadsheets/d/YOUR_SPREADSHEET_ID/edit
   ```

3. **Create the n8n volume**

   ```bash
   docker volume create n8n_data
   ```

4. **Start all services**

   ```bash
   docker-compose up -d
   ```

5. **Access the applications**
   - **Web App**: <http://localhost:3000> (Upload PDFs and images of receipts/statements)
   - **n8n Dashboard**: <http://localhost:5678> (Configure workflows and credentials)

6. **Configure n8n credentials**
   - Go to <http://localhost:5678>
   - Import the workflow from `/n8n/ai-expense-tracker-n8n.json`
   - Set up Google Drive OAuth2 and Google Sheets OAuth2 credentials
   - Use the redirect URI: `http://localhost:5678/rest/oauth2-credential/callback`

7. **Upload your first expense**
   - Go to <http://localhost:3000>
   - Upload a bank statement PDF or receipt image (JPG, PNG)
   - Watch it automatically appear in your Google Sheet!

📖 **Need help with Docker?** Check out the [Docker Guide](DOCKER-GUIDE.md) for detailed explanations.

### Managing Your Docker Application

```bash
# Stop all services
docker-compose down

# View logs
docker-compose logs -f

# View logs for specific service
docker-compose logs -f web-app
docker-compose logs -f n8n

# Restart services
docker-compose restart

# Rebuild and restart (after code changes)
docker-compose up --build -d

# Stop and remove everything (including volumes - ⚠️ deletes n8n data)
docker-compose down -v
```

---

## ⚙️ Alternative Setup (Manual n8n)

If you prefer to run n8n manually without Docker:

### Prerequisites

- [n8n](https://n8n.io/) installed locally.
- A [Google Cloud Project](https://console.cloud.google.com/) with:
  - **Google Drive API** enabled.
  - **Google Sheets API** enabled.
- A Google Sheet based on the provided template (`/templates/balance.xlsx`).

### Steps

1. **Clone the repository**

   ```bash
   git clone https://github.com/matiasvallejosdev/ai-expense-tracker-n8n.git
   cd ai-expense-tracker-n8n
   ```

2. **Import the workflow**
   - Go to your local [n8n dashboard](http://localhost:5678).
   - Import `/n8n/ai-expense-tracker-n8n.json`.
3. **Configure credentials**
   - In *n8n → Credentials*, create:
     - `Google Drive OAuth2`
     - `Google Sheets OAuth2`
   - Use the redirect URI:

     ```
     http://localhost:5678/rest/oauth2-credential/callback
     ```

   - Follow Google’s guide for OAuth setup:
     [Create OAuth Client ID](https://developers.google.com/workspace/guides/create-credentials#oauth-client-id)
4. **Connect your template**
   - Upload the provided `/templates/balance.xlsx` to your Google Drive.
   - Rename it if you wish, but keep a tab named **“Expenses Model”**.
5. **Set your folder trigger**
   - In the workflow, edit the node **Google Drive Trigger** and replace the folder ID with your own.
6. **Run it**
   - Upload a PDF statement into that folder.
   - Watch your Google Sheet auto-populate with parsed transactions!

### 🗂️ Google Drive Folder (Input)

This is where you should upload your bank statement PDFs.
📁 [Open the Drive Folder](https://drive.google.com/drive/folders/16G0cnl6PA_ds68Q-sj4-E71sdmt8K0Jp?usp=sharing)

Each file you drop there (e.g. `statement_visa.pdf`) will trigger the workflow and automatically process it.

### 📊 Google Sheets Template (Output)

All categorized data will be appended automatically to this Google Sheets template:
📈 [Open the Google Sheets Template](https://docs.google.com/spreadsheets/d/1CxrFIACqAUA7uu7fivSMhzzHtaIfcpxcIVr4NxnCObo/edit?usp=sharing)

Or click **[Make a Copy](https://docs.google.com/spreadsheets/d/1CxrFIACqAUA7uu7fivSMhzzHtaIfcpxcIVr4NxnCObo/copy)** to create your own editable version.

## 🧩 Detailed Setup (Developers)

### Tech Stack

**Backend (n8n Workflow)**

- n8n workflow automation
- OpenAI GPT-4o for text extraction and categorization
- Google Drive API
- Google Sheets API

**Frontend (Web App)**

- Next.js 16 (React 19)
- TypeScript
- Tailwind CSS 4
- Shadcn/ui components
- React Hook Form + Zod validation
- Docker multi-stage builds

### Customization

1. **Customize Agents**
   - `AI Agent` → parses raw text from PDF/image.
   - `AI Agent – Structure Data` → converts it into a valid JSON array.

2. **Processing Flow**
   - Web Upload/Google Drive Trigger → Download File → Extract Text (OCR for images) → AI Parse → JSON Normalize → Filter Transactions → Append to Sheet.

3. **Script Filters**
   - Automatically ignores tax lines (`IVA`, `Percepciones`, `Impuesto de Sellos`, etc.) and USD duplicates.

4. **Web App Development**

   ```bash
   cd web-app
   npm install
   npm run dev
   ```

   The app will be available at <http://localhost:3000>

5. **Output Schema**

   | Field            | Type   | Description                           |
   | ---------------- | ------ | ------------------------------------- |
   | `Month`          | string | Month extracted from transaction date |
   | `Date`           | string | ISO date (YYYY-MM-DD)                 |
   | `Concept`        | string | Transaction detail                    |
   | `Payment_Method` | string | Credit Card / Debit Card / Cash       |
   | `Category`       | string | Expense category                      |
   | `Amount`         | number | Value of transaction                  |
   | `Currency`       | string | ARS / USD                             |

## 📊 Data Model

**Fixed Taxonomy**

| **Currency** | **Payment Method**            | **Expense Categories**                                                                          | **Income Categories**                            |
| ------------ | ----------------------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| ARS, USD     | Credit Card, Debit Card, Cash | Supermarket, Gastronomy, Services, Fashion, Transport, Technology, Home, Education, Sports, ... | Salary, Freelancing, Investment, Business, Extra |

You can extend these categories directly in the **AI prompts** or in the **Google Sheet data validation lists**.

## 🤖 Architecture Overview

### Input Methods

**Option 1: Web App (Next.js)**

1. User uploads file via web interface (<http://localhost:3000>)
2. File sent to n8n webhook endpoint
3. n8n processes the file

**Option 2: Google Drive Trigger**

1. User drops file in Google Drive folder
2. n8n watches folder and detects new files
3. n8n downloads and processes the file

### Processing Pipeline (Both Methods)

1. **File Input** → Web upload or Google Drive trigger
2. **Extract File Data** → Converts PDF/image to text using OCR and AI
3. **Data Parser & Cleaner** → Formats the raw text
4. **AI Agent (GPT-4o)** → Extracts transactions from text
5. **AI Agent – Structure Data** → Converts to JSON array
6. **Parse JSON Output** → Validates structure
7. **Aggregate → Duplicate Sheet → Append to Sheet** → Final output in your balance template

> Everything runs locally on your machine through Docker — your data stays 100% private.

### Docker Services

- **n8n** (port 5678) – Workflow automation engine
- **web-app** (port 3000) – Next.js frontend for file uploads

Both services communicate over a Docker bridge network.

## 💡 Usage

### Using the Web App (Recommended)

1. **Open the web interface** at <http://localhost:3000>
2. **Upload your expense document**:
   - ✅ Bank statement PDFs
   - ✅ Receipt images (JPG, PNG)
   - ✅ Ticket photos from your phone
3. **Wait for AI processing** (usually 5-10 seconds)
4. **Check your Google Sheet** – new transactions appear automatically!

### Using Google Drive Trigger (Alternative)

- Upload PDF → Wait for execution → Open your **Google Sheet**.
- The system auto-creates a new sheet for each month:

  ```
  Expenses Oct 2025
  ```

- Filter, analyze, or connect to dashboards.

- Works perfectly with the included balance template dashboard.

## 🤝 Contributing

Contributions are welcome!
If you want to improve parsing logic, add new categories, optimize the workflow, or enhance the web app:

1. Fork the repository.
2. Create a feature branch.
3. Make your changes:
   - **n8n workflows**: Edit JSON files in `/n8n/`
   - **Web app**: Edit files in `/web-app/`
   - **Docker setup**: Update `docker-compose.yml` or `Dockerfile`
4. Test your changes with Docker:

   ```bash
   docker-compose up --build
   ```

5. Commit and push your changes.
6. Open a Pull Request.

**Guidelines:**

- Include a **redacted PDF/image example** if your update changes extraction logic
- Test both input methods (web app and Google Drive) if modifying the processing pipeline
- Update the README if you add new features or change setup steps

## 📞 Contact

If you have questions, suggestions, or want to collaborate:

- **Name:** Matías Vallejos
- 🌐 [matiasvallejos.com](https://matiasvallejos.com)
- 𝕏 [@mativallejosdev](https://x.com/mativallejosdev)

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🧠 Inspiration

> “Control your money, or your money will control you.” — *La Bolsa o la Vida*

> “Start thy purse to fattening.” — *El Hombre Más Rico de Babilonia*
