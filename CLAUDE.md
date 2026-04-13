# OTR DC Site Visit App

## Overview
A single-file mobile-first web app for conducting OTR Distribution Centre site visits. Auditors fill in metadata, answer 6 structured questions with typed notes and photos, then export a PDF or send via email.

## Architecture
- **Single file**: `index.html` — all HTML, CSS, and JavaScript inline. No build step, no dependencies to install.
- **Tailwind CSS** via CDN (`https://cdn.tailwindcss.com`)
- **jsPDF** via CDN (`https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js`) for PDF generation

## Key Features
- **Metadata fields**: DC/Site Name, Auditor Name, Visit Date (auto-populated)
- **6 question cards**: Each has a textarea for notes and a multi-photo attachment area
- **Photo handling**: Photos are compressed to max 1200px (JPEG 75%) via canvas before storage. Stored in-memory only (not localStorage) to avoid storage quota issues.
- **Auto-save**: Text answers and metadata persist to `localStorage` under key `otr_dc_site_visit_v1`. Photos are lost on page refresh by design.
- **PDF export**: Programmatically built with jsPDF — includes header, metadata, all Q&A, and embedded photos at correct aspect ratios. Pages added automatically as content overflows.
- **Email**: Opens `mailto:` with subject and plain-text body pre-filled. User manually attaches the downloaded PDF for photos.

## How to Add or Modify Questions
Edit the `QUESTIONS` array in the `<script>` block:
```js
const QUESTIONS = [
  { short: "Short Label",  text: "Full question text shown to the auditor." },
  // ...
];
```
- `short` — displayed as a coloured subtitle and used in the PDF/email as a section heading
- `text` — the full question shown on the card

Adding or removing questions automatically updates the progress bar, PDF output, and email body.

## How to Change the Email Recipient
In the `sendEmail()` function, update the `mailto:` line:
```js
const mailto = `mailto:recipient@example.com?subject=...`
```

## Styling Notes
- Max width `430px`, centred — optimised for phone screens
- Brand colour: `#2563eb` (blue-600)
- Custom CSS classes (`question-card`, `meta-input`, `btn-primary`, etc.) are defined in the `<style>` block
- Avoid using Tailwind for layout of custom components — use the inline style classes for consistency
