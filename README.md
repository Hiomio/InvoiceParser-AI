# LLM-Based Invoice OCR

A hybrid Invoice OCR pipeline that extracts structured JSON data from invoices using OCR + LLMs. This project demonstrates an end-to-end intelligent document processing workflow with support for both paid API-based extraction and open-source OCR fallback.

## Demo

🎬 Demo Video:
https://drive.google.com/file/d/17MpFdzs3mrm-jd801CzsPZ-NdsQOmw6s/view?usp=drive_link

---

## Features

* Accepts PDF or image uploads
* Supports multi-page PDF processing
* Converts PDFs into images using `pdf2image`
* Extracts structured invoice data such as:

  * Invoice Number
  * Vendor Name
  * Invoice Date
  * Line Items
  * Subtotal
  * Tax
  * Grand Total
  * Payment Terms
  * Billing Details
* Returns clean structured JSON output
* Supports both:

  * Paid API Mode
  * Open-Source OCR Mode
* Includes sample invoices for testing and demo purposes

---

## Tech Stack

### Backend

* FastAPI

### Frontend

* Gradio

### OCR

* pdf2image
* Tesseract OCR (`pytesseract`)
* Together AI API

### Language Model

* Qwen2.5-VL-72B-Instruct

---

## Extraction Modes

### Paid Mode

Uses Together AI API with Qwen2.5-VL-72B-Instruct for high-accuracy invoice extraction directly from invoice images.

### Open-Source Mode

Uses Tesseract OCR with rule-based heuristics for a completely free fallback option.

---

## Installation

### Step 1: Create Virtual Environment

### Windows (PowerShell)

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1

pip install --upgrade pip
pip install -r requirements.txt
```

### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate

pip install --upgrade pip
pip install -r requirements.txt
```

---

## Step 2: Install Required Tools

### Poppler (Required for PDF to Image Conversion)

### Windows

Download from:
https://github.com/oschwartz10612/poppler-windows/releases

Example path:

```text
C:\poppler\poppler-23.08.0
```

Make sure the `Library/bin` folder exists.

### macOS

```bash
brew install poppler
```

### Ubuntu / Debian

```bash
sudo apt install poppler-utils
```

---

### Tesseract OCR (Recommended for Open-Source Mode)

### Windows

Install from:
https://github.com/tesseract-ocr/tesseract

Example path:

```text
C:\Program Files\Tesseract-OCR\tesseract.exe
```

Ensure Tesseract is added to PATH.

### macOS

```bash
brew install tesseract
```

### Ubuntu / Debian

```bash
sudo apt install tesseract-ocr
```

---

## Step 3: Configure Environment Variables

Copy `.env.example` to `.env`

```env
TOGETHER_API_KEY="your_api_key_here"
TOGETHER_MODEL="Qwen/Qwen2.5-VL-72B-Instruct"
TOGETHER_INFERENCE_URL="https://api.together.xyz/v1/chat/completions"
```

---

## Optional Windows Configuration

### Set POPPLER_PATH

If Poppler is not added to PATH:

```powershell
$env:POPPLER_PATH = "C:\poppler\poppler-23.08.0\Library\bin"
```

You can also set it permanently via Environment Variables.

---

### Set Tesseract Path Manually (if needed)

```python
pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

---

## Run the Application

### Start Backend

```powershell
uvicorn src.backend.main:app --reload
```

Backend runs on:

```text
http://127.0.0.1:8000
```

---

### Start Frontend

In another terminal:

```powershell
python src/frontend/gradio_app.py
```

Frontend usually runs on:

```text
http://127.0.0.1:7860
```

Use sample files from:

```text
sample_invoices/
```

---

## Sample JSON Output

```json
{
  "invoice_number": "INV-2025-104",
  "vendor_name": "ABC Supplies Pvt Ltd",
  "invoice_date": "2025-03-12",
  "line_items": [
    {
      "description": "Office Chairs",
      "quantity": 10,
      "unit_price": 2500,
      "total": 25000
    }
  ],
  "grand_total": 29500
}
```

---

## Troubleshooting

### Poppler Not Found

Make sure:

* Poppler is installed correctly
* `POPPLER_PATH` points to `Library/bin`

---

### Tesseract Not Found

Either:

* Add Tesseract to PATH

OR

* Set the full path manually in code

---

### Paid Mode Returns 401 / 403

Verify:

```env
TOGETHER_API_KEY
```

Restart the backend after updating `.env`

---

### Frontend Cannot Connect to Backend

Ensure FastAPI backend is running before starting Gradio.

---

### Poor OCR Quality

Use:

* Better quality invoice scans
* Higher resolution PDFs

(Default conversion DPI is 300)

---

## Future Improvements

* Excel / CSV export
* Vendor-specific invoice templates
* Database integration
* Human-in-the-loop review system
* Docker support
* AWS / GCP deployment
* Invoice validation workflows

---

## Author

Built as an Intelligent Document Processing project using OCR + LLM pipelines for real-world invoice automation.
