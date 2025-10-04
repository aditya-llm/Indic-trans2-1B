---
i have to edit line 52-56

````markdown
# NCERT PDF Translator (English to Hindi) 📖

This project provides a Python script to automatically download NCERT textbook chapters,
extract their content, translate the text from English to Hindi, and recreate the PDFs
while preserving the original layout, including images and text positions.

It uses a sophisticated pipeline involving web scraping, advanced PDF parsing, a state-of-the-art
machine translation model, and programmatic PDF generation.


*(A high-level overview of the process)*

---

## ✨ Features

-   **⬇️ Automatic Downloader:** Fetches specified NCERT chapter PDFs directly from the official website.
-   **🖼️ Layout-Aware Extraction:** Uses `PyMuPDF` to extract not just text, but also its exact position, font size, and images.
-   **🤖 High-Quality Translation:** Leverages the powerful `ai4bharat/indictrans2-en-indic-1B` model for
            accurate English-to-Hindi translation.
-   **📝 PDF Reconstruction:** Generates a new Hindi PDF from scratch using `reportlab`,
            placing translated text and original images in their original locations to mirror the source layout.
-   **🧹 Noise Removal:** Includes basic text cleaning to remove common artifacts like headers and footers.
-   **⚡ Batch Processing:** Translates text in batches for significantly improved performance, especially on a GPU on Google colab.

---

## ⚙️ How It Works

The script follows a five-step process for each PDF file:

1.  **Download:** The script downloads the specified English PDF chapters from the NCERT website.
2.  **Extract:** It opens the PDF and meticulously extracts every piece of content page by page.
        This includes text blocks with their coordinates (`x`, `y`), font size, and any images with their bounding boxes.
3.  **Collect & Translate:** All extracted English text snippets are collected and sent to the IndicTrans2 model in efficient batches.
        The model returns the Hindi translations.
4.  **Rebuild:** The script creates a new, blank PDF. It then iterates through the extracted content structure.
    -   If an item is an **image**, it's drawn onto the new PDF at its original coordinates.
    -   If an item is **text**, the corresponding Hindi translation is drawn at its original coordinates using a specified Hindi font.
5.  **Save:** The final, translated Hindi PDF is saved to the output directory.

---

## 🚀 Getting Started

### Prerequisites

-   Python 3.8+
-   **A Hindi Unicode Font:** The script needs a `.ttf` font file to write Hindi text in the PDF. We recommend **Noto Sans Devanagari**.
    -   [**Download it from Google Fonts here**](https://fonts.google.com/noto/specimen/Noto+Sans+Devanagari).
    -   After unzipping, find the `NotoSansDevanagari-Regular.ttf` file and place it in the same directory as the Python script.
-   **(Highly Recommended) A CUDA-enabled GPU:** The translation model is very large (~4GB). Running it on a CPU will be **extremely slow**. A GPU will speed up the translation process by orders of magnitude.

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/ncert-pdf-translator.git
    cd ncert-pdf-translator
    ```

2.  **Install the required Python libraries:**
    ```bash
    # For PDF manipulation, web requests, and PDF creation
    pip install PyMuPDF requests Pillow reportlab tqdm

    # For the AI translation model (PyTorch, Transformers, etc.)
    pip install transformers torch sentencepiece accelerate
    ```
    *Note: If you have a CUDA GPU, make sure you have the correct version of PyTorch installed. See the [official PyTorch website](https://pytorch.org/get-started/locally/) for instructions.*

---

## 🏃‍♀️ Usage

1.  **Place the Font File:** Ensure the `NotoSansDevanagari-Regular.ttf` file is in the root directory of the project. The script is configured to look for this file.

2.  **Run the script from your terminal:**
    ```bash
    python translate_ncert.py or you can use colab translate_ncert.ipynb in colab.
    ```

3.  **Wait for the process to complete.** The script will perform the following actions:
    -   First, it will download the English PDFs into a folder named `english_pdfs/`.
    -   Next, it will download and initialize the translation model. This might take some time
        and will download several gigabytes of data on the first run.
    -   It will then process each PDF, showing progress bars for extraction, translation, and PDF creation.
    -   The final translated files will be saved in the `hindi_pdfs/` directory with `_hindi.pdf` appended to their names.

### Project Structure

```
.
├── translate_ncert.py             # The main Python script
├── NotoSansDevanagari-Regular.ttf # The required Hindi font file
├── english_pdfs/                  # Folder for downloaded original PDFs
├── hindi_pdfs/                    # Folder for generated translated PDFs
└── README.md                      # This file
```

---

## ⚠️ Limitations and Considerations

-   **Performance:** Translation is computationally expensive. **A GPU is strongly recommended.**
        A single chapter might take a very long time on a CPU.
-   **Layout Fidelity:** The layout is a close approximation, not a pixel-perfect replica.
        Because translated text can be longer or shorter than the original, some text may overflow
        its original bounding box. The script does not currently handle automatic text wrapping.
-   **Complex Tables:** While the script handles simple text blocks well, it may not perfectly
        reconstruct complex tables or intricate layouts.
-   **Font Styles:** The script recreates text using a single, regular font style. It does not
        preserve **bold**, *italics*, or other font variations from the original document.

---

## 🛠️ Technologies Used

-   **PDF Extraction:** [PyMuPDF (Fitz)](https://github.com/pymupdf/PyMuPDF)
-   **Machine Translation:** [Hugging Face Transformers](https://huggingface.co/docs/transformers/index)
        with the [ai4bharat/indictrans2-en-indic-1B](https://huggingface.co/ai4bharat/indictrans2-en-indic-1B) model.
-   **PDF Creation:** [ReportLab](https://www.reportlab.com/opensource/)
-   **HTTP Requests:** [Requests](https://requests.readthedocs.io/en/latest/)
-   **Deep Learning Framework:** [PyTorch](https://pytorch.org/)

---

## 📝 Acknowledgements

-   **NCERT** for providing high-quality educational materials.
-   **AI4Bharat** for creating and open-sourcing the powerful IndicTrans2 translation models.

---
