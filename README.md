# 🧾 InvoiceParser AI

A hybrid Invoice OCR pipeline that extracts structured JSON data from invoices. This project demonstrates an end-to-end intelligent document processing pipeline.

## 🎬 Demo

📹 [Watch Demo](https://drive.google.com/file/d/17MpFdzs3mrm-jd801CzsPZ-NdsQOmw6s/view?usp=drive_link)

---

## ✨ Features

- 📄 Accepts **PDF or image uploads** (`pdf2image` used to convert PDFs)
- 📑 **Multi-page PDFs** supported (each page converted to an image and processed)
- 🔍 Extracts structured data: `invoice_number`, `vendor_name`, `invoice_date`, `line_items`, `grand_total`, etc.
- 🔀 Switch between **Paid API** and **Open-source OCR** modes
- 📦 Returns clean, structured **JSON**
- 🧪 Sample invoices included for testing/demo

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | FastAPI |
| Frontend | Gradio |
| OCR (open-source) | pdf2image + Tesseract |
| OCR (paid) | Together AI — `Qwen2.5-VL-72B-Instruct` |

### Modes

| Mode | Description |
|------|-------------|
| `paid` | Uses Together AI (`Qwen2.5-VL-72B-Instruct`) to extract structured JSON from invoice images |
| `open_source` | Uses `pytesseract` OCR + heuristics as a free fallback option |

---

## 🚀 Quickstart

### 1. Create a virtual environment and install dependencies

**Windows (PowerShell):**
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install --upgrade pip
pip install -r requirements.txt
```

**macOS/Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

---

### 2. Install prerequisites

#### Poppler *(required for PDF → image conversion)*

| OS | Command |
|----|---------|
| Windows | Download from [poppler-windows releases](https://github.com/oschwartz10612/poppler-windows/releases), unzip to `C:\poppler\poppler-23.08.0`, ensure `Library\bin` exists |
| macOS | `brew install poppler` |
| Ubuntu/Debian | `sudo apt install poppler-utils` |

#### Tesseract *(recommended for open-source OCR mode)*

| OS | Command |
|----|---------|
| Windows | Install from [tesseract-ocr](https://github.com/tesseract-ocr/tesseract), ensure it's added to PATH |
| macOS | `brew install tesseract` |
| Ubuntu/Debian | `sudo apt install tesseract-ocr` |

---

### 3. Configure environment variables

Copy `.env.example` to `.env` and set values as needed *(only the API key is required for paid mode)*:

```env
TOGETHER_API_KEY="your_api_key_here"
TOGETHER_MODEL="Qwen/Qwen2.5-VL-72B-Instruct"
TOGETHER_INFERENCE_URL="https://api.together.xyz/v1/chat/completions"
```

> **Windows only:** If Poppler is not on PATH, set `POPPLER_PATH`:
> ```powershell
> # Current session
> $env:POPPLER_PATH = "C:\\poppler\\poppler-23.08.0\\Library\\bin"
> ```
> Or set it permanently via **System Properties → Environment Variables**.

> **Tesseract (open_source mode):** Ensure Tesseract is on PATH, or manually set:
> ```python
> pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
> ```

---

### 4. Run the backend (FastAPI)

```powershell
uvicorn src.backend.main:app --reload
```

---

### 5. Run the frontend (Gradio)

In a **second terminal** (with the same venv activated):

```bash
python src/frontend/gradio_app.py
```

Open the Gradio UI at the URL printed in the terminal (typically `http://127.0.0.1:7860`). Try files from `sample_invoices/`.

---

## 🐛 Troubleshooting

| Issue | Fix |
|-------|-----|
| **Poppler not found / 500 error converting PDFs** | Ensure Poppler is installed and `POPPLER_PATH` points to its `Library\bin` folder (Windows) |
| **Tesseract not found in `open_source` mode** | Add Tesseract to PATH or set `pytesseract.pytesseract.tesseract_cmd` to its full path |
| **401/403 errors in `paid` mode** | Verify `TOGETHER_API_KEY` in `.env` and restart the backend |
| **Connection failed between frontend and backend** | Confirm backend is running at `http://127.0.0.1:8000` before starting the frontend |
| **Poor OCR quality (`open_source`)** | Try higher DPI scans — backend uses 300 DPI for PDF conversion by default |
| **Multi-page PDFs** | Each page is processed and aggregated; response includes `pages` count |

---

## 📁 Project Structure

```
.
├── src/
│   ├── backend/
│   │   └── main.py          # FastAPI app
│   └── frontend/
│       └── gradio_app.py    # Gradio UI
├── sample_invoices/         # Sample PDFs/images for testing
├── .env.example
├── requirements.txt
└── README.md
```

---

## 📄 License

MIT License
