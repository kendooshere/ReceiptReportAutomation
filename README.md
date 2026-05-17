# 📊 Receipt Report Automation

Automates the end-to-end extraction, data-cleansing, and target synchronization of daily commercial leasing receipt reports. By replacing manual administrative reporting with an automated data pipeline, this utility bridges the gap between enterprise lease ledger data and real-time management dashboards.

---

## 🛠️ Tech Stack

* **Automation:** Python, Selenium WebDriver
* **Data Engineering:** Pandas
* **Target Pipeline:** Google Sheets API (`gspread`), Google Service Accounts (`google-auth`)
* **Environment Management:** Dotenv (Secure configuration isolation)

---

## 🚀 System Architecture

```text
[Lease Management Portal] ──( Selenium Scripted DOM Action )──> [ Raw Multi-Tab .xlsx Download ]
                                                                             │
                                                                    ( Pandas Vectorized Pipeline )
                                                                             │
[Google Sheets (Subsheet 2)] <──( USER_ENTERED Injection )───────── [ Sanitized Formula-Ready Array ]

```

### Key Workflow States

* **Scripted Ledger Navigation:** Launches an isolated automated Chrome instance, logs securely into the leasing portal, configures explicit report date fields asynchronously, and exports historical ledger records.
* **Financial Data Sanitization:** Runs raw downloads through a Pandas extraction layer that handles inconsistent data frames, strips mathematical noise from currency entries, and standardizes multi-region datetimes.
* **Downstream Formula Integration:** Leverages the Google Sheets API to wipe staging sheets and cleanly load pristine structural arrays using execution-level parsing parameters.

---

## 💻 Technical Features & Implementation

### ⚡ Asynchronous JavaScript Injection

Bypasses rigid, click-heavy UI date-pickers by injecting raw text directly into the `FromDate` and `ToDate` elements via custom execution scripts. It triggers proper DOM change and blur events natively:

```javascript
arguments[0].value = arguments[1];
arguments[0].dispatchEvent(new Event ('change'));
arguments[0].dispatchEvent(new Event ('blur'));

```

### 📈 Downstream Formula-Ready Normalization

* **Numeric Typecasting:** Automatically strips formatting strings, commas, and spaces from currency cells, casting them back to numeric floats to ensure `SUMIFS` and pivot metrics compile flawlessly in Google Sheets.
* **ISO 8601 Temporal Uniformity:** Forces all auditing columns (`Created Date`, `Transaction Date`, `Chq Date`, `PDC Matured On`) into strict `YYYY-MM-DD` compliance to bypass cross-regional parsing glitches (e.g., UK vs. Indian locale date discrepancies).

### 🛡️ Fail-Safe Infrastructure

* **Download Tracking Engine:** Monitors the local directory using a dynamic polling loop that tracks pending `.crdownload` system markers, accurately waiting out long-running exports before passing data to the parser.
* **JSON-Compliant Sanitization:** Replaces system `NaN` and infinity flags with clean empty strings (`""`) to avoid payload errors during API transit.
* **Sheet Context Preservation:** Pushes updates directly using `USER_ENTERED` values, stripping out text-identifying `'` apostrophe prefixes so Google Sheets handles numbers and dates natively instead of treating them as raw strings.
* **Clean Session Containment:** Enforces a structural `try-except-finally` block that guarantees the browser instance safely runs `driver.quit()`, terminating leaking background processes even on network timeouts.

---

## 📂 Project Structure

```plaintext
├── .env                  # Local environment configuration (Ignored by Git)
├── main.py               # Principal automation pipeline script
├── requirements.txt      # Project-level dependencies
└── README.md             # Documentation

```

### Environment Variables (`.env`)

To run this pipeline locally, create a `.env` file in the root directory matching this schema:

```ini
USER_NAME=your_portal_username
PASS_WORD=your_portal_password
PORTAL_URL=https://your-lease-portal.com
CREDENTIALS=path/to/google_service_account.json
BUFFER_SHEET=google_spreadsheet_id_key
DOWNLOAD_PATH=path/to/local/download/directory

```

---

## 💎 Business Impact

* **⏱️ Operational Efficiency:** Completely eliminates 15–20 minutes of daily tedious clicking, manual folder sorting, file conversion, and row patching.
* **🎯 Data Integrity:** Removes human copy-paste errors, ensuring critical downstream financial dashboards ingest perfectly uniform, pristine structural arrays.
* **⚡ Reporting Agility:** Empowers senior management and review teams with instantly accessible, real-time receipt visibility directly on their centralized dashboards.
