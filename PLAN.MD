# PDF-iler Design Document

## Overview
PDF-iler is an application designed to process large quantities of PDFs and images in bulk, allowing users to create templates that map to specific data points within the documents. The app then uses these templates to extract and organize data from other similar documents, outputting it in structured formats like CSV, JSON, or directly into a database.

This project is aimed at automating the extraction of data from a variety of documents, including PDFs and images (scans, photos), to increase the speed and accuracy of data processing. By digitizing printed or handwritten documents, we can improve data accessibility and streamline workflows.

---

## Functional Requirements

The app consists of two primary modules:
1. **Template Builder**: A visual interface for creating and saving templates by interacting with PDFs or images.
2. **Parser**: A tool that uses templates to extract data from multiple files in bulk, outputting structured data in formats such as CSV, JSON, or other file types.

### 1. Template Builder

#### Features:
1. **Visual Interface for Loading Files**:
   - Users can upload PDFs or images (including scanned or handwritten documents) to begin creating templates.
   - Support for image orientation and normalization using visual anchors like logos or unique key markers.

   **Options**:
   - **React File Input**: Use HTML `<input type="file">` with drag-and-drop capabilities for easier file loading.
   - **Libraries**:
     - `react-pdf` for rendering PDF documents.
     - `tesseract.js` for OCR (Optical Character Recognition) to handle images.

2. **Mouse Interaction for Defining Areas**:
   Users can visually select:
   - **Anchor Points**: Use known reference points (like logos or company names) to scale and orient the document.
   - **General Areas**: Identify larger sections (e.g., headers, footers).
   - **Specific Targets**: Select individual data labels (e.g., "Subtotal", "Amount Due").
   - **Expected Data Areas**: Highlight where the actual data (e.g., numbers or text) should be extracted.
   - **Target Groups**: Link multiple areas together to define relationships between them.

   **Options**:
   - **Canvas Interaction**: 
     - Use an HTML canvas to enable area selection.
     - Libraries like `fabric.js` or `Konva.js` to manage user interaction with the document visually.
   - **Bounding Boxes**: Users can create bounding boxes around target areas and label them appropriately (e.g., for total amounts, names, dates).

3. **Identify Data Types**:
   After selecting target areas, users can categorize them (e.g., names, dates, totals) to ensure correct extraction and formatting.

   **Options**:
   - Predefined categories or custom labels.
   - Regex support for auto-classifying simple fields such as numbers and dates.

4. **Saving Templates**:
   Templates will include all selected areas, data types, and anchor points, saved in JSON format for future use. This allows users to quickly load templates for similar document types.

   **Options**:
   - **JSON-based Templates**: Ensure easy reusability and portability.
   - Local file storage or cloud storage options (if scaling to multiple users or devices).

5. **Dataset Integration**:
   When creating new templates, users will be able to assign them to a specific dataset (e.g., billing, environmental reports). This ensures consistency between different documents by keeping field names standardized across the same dataset.

   **Options**:
   - A list of predefined fields for each dataset to avoid duplication (e.g., ensuring "Address" is consistent across different forms).
   - Warnings for potential column mismatches (e.g., "address" vs. "Address").

---

### 2. Parser

#### Features:
1. **Select Existing Templates**:
   Users can select previously saved templates to use for parsing new files, ensuring consistent data extraction across similar document sets.

   **Options**:
   - File explorer or dropdown to select available templates.

2. **Batch Processing**:
   Users can upload one or more files (PDFs or images) and apply the selected template to extract data from them in bulk.

   **Options**:
   - Support for bulk uploads via drag-and-drop or a file/folder selector.
   - Integrations with cloud storage services (Google Drive, Dropbox) for easier batch uploads.

3. **Template Matching**:
   The app will identify the correct template to use by detecting reference elements (e.g., logos, company names) within the document. This will also handle document orientation corrections.

   **Options**:
   - Use **Tesseract.js** for OCR and pattern matching.
   - Use **OpenCV** for image recognition and orientation adjustments.

4. **Data Extraction**:
   The app uses the template to parse predefined fields. If the data is in a PDF, it will attempt to extract text directly; if it's an image, OCR will be applied.

   **Options**:
   - Use `pdf-lib` or `pdf.js` for parsing PDFs.
   - Apply OCR using **Tesseract.js** for images.
   - Use regex for structured parsing of specific data formats (e.g., dates, totals).
   - Transformation functions for cleaning and formatting data (e.g., removing extra characters, correcting date formats).

5. **Output**:
   Extracted data will be output in structured formats like CSV or JSON, including metadata for each file processed (e.g., file name, timestamp, confidence score, errors).

   **Options**:
   - **Primary Data File**: Contains the requested fields and metadata.
   - **Metadata File**: Additional file for storing process details (e.g., skipped files, confidence values, errors).

---

### 3. Data Validation

#### Features:
1. **Confidence Value**:
   Each extracted field will be assigned a confidence score, indicating the likelihood that the parsed data is correct.

   **Options**:
   - OCR confidence from Tesseract.
   - Pattern matching for structured data (e.g., number matching for totals).

2. **Random Sampling**:
   To ensure data accuracy, a random selection of processed files will be flagged for manual validation. This feature can be adjusted based on user feedback or app confidence scores.

   **Options**:
   - Random sample rate (e.g., 1% of files).
   - Automatic adjustment based on validation outcomes.

---

### 4. Scalability Considerations

#### Features:
1. **Performance Optimization**:
   For large batches, performance optimizations will be necessary, such as breaking tasks into smaller chunks or using background workers.

   **Options**:
   - **Web Workers**: Offload parsing tasks to background threads.
   - **Batch Processing**: Show progress bars and batch results to keep the app responsive.

2. **Cloud Integration**:
   If scaling to large datasets, integrate cloud-based OCR or processing services (e.g., Google Vision API, AWS Textract) for faster performance.

   **Options**:
   - Handle both PDFs and images seamlessly with standardized parsing logic.

---

## Conclusion
PDF-iler will be a comprehensive tool designed to streamline the processing of large batches of PDFs and images, extracting data efficiently through template-based parsing. By implementing features such as OCR, pattern recognition, confidence scoring, and data validation, it will ensure accuracy and scalability, supporting diverse document formats and datasets.
