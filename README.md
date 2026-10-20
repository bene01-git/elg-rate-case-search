# NY DPS Rate Case Document Scraper

Scrapes documents from the New York Department of Public Service (DPS) Document and Matter Management (DMM) system and appends them to the `ELG_Database.xlsx` workbook.

## Setup

### 1. Install Python 3.9+

Download from [python.org](https://www.python.org/downloads/) if you don't have it.

### 2. Install Google Chrome

The scraper uses Chrome. Download from [google.com/chrome](https://www.google.com/chrome/) if needed.

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

This installs:
- **selenium** – browser automation for scraping the JavaScript-heavy DPS site
- **openpyxl** – reads/writes Excel files
- **webdriver-manager** – automatically downloads the correct ChromeDriver

### 4. Place your Excel file

Put `ELG_Database.xlsx` in the same folder as `dps_scraper.py`, or specify its path with `--output`.

## Usage

### Basic — by case number
```bash
python dps_scraper.py 23-G-0225
```

### By full URL
```bash
python dps_scraper.py "https://documents.dps.ny.gov/public/MatterManagement/CaseMaster.aspx?MatterCaseNo=23-G-0225"
```

### Custom output path
```bash
python dps_scraper.py 23-G-0225 --output /path/to/ELG_Database.xlsx
```

### Debug mode (visible browser)
```bash
python dps_scraper.py 23-G-0225 --visible
```

## What it captures

For each document in the case, a row is added to the spreadsheet with:

| Column | Description |
|--------|-------------|
| Case Number | e.g. `23-G-0225` |
| Company Name | e.g. `National Fuel Gas Distribution Corporation` |
| Company Type | `Gas`, `Electric`, or `Electric and Gas` |
| Date Filed | Filing date from the document grid |
| Document Type | e.g. `Orders`, `Testimony`, `Letter` |
| Document Title | Full title of the document |
| Filing on Behalf Of | The party that filed the document |
| File Name | The attachment file name |
| File Size | e.g. `541 KB` |
| Link to Document | Clickable hyperlink to the document on DPS |

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "Case number does not exist" | Double-check the case number on the DPS website manually |
| Grid doesn't load | Run with `--visible` to watch the browser; the site may be slow |
| ChromeDriver errors | Ensure Chrome is installed; `webdriver-manager` should auto-download the right driver |
| No documents found | The "Filed Documents" tab may load slowly; try again or use `--visible` |
| Permission error on Excel | Make sure the file isn't open in Excel |

## Notes

- The DPS site is ASP.NET with heavy JavaScript — the scraper uses Selenium to render pages fully before extracting data.
- Large cases (1000+ documents) may take several minutes as the scraper pages through the grid.
- The scraper appends to the existing sheet; it does not overwrite previous data.
- Run multiple times with different case numbers to build up your database.
