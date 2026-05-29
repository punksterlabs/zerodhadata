# Zerodha Data (DAS)

Automated Data Acquisition System for Nifty 500 stocks, Nifty Options, and BankNifty Options via Zerodha websocket.

## What It Does

Fully automated pipeline that logs into Zerodha daily, subscribes to all Nifty 500 stocks plus weekly options for Nifty and BankNifty (current and next expiry), receives real-time tick data via websocket, stores it in MySQL/MariaDB, and performs end-of-day backup into individual instrument tables. Handles trading holiday checks, dynamic Nifty 500 list updates, automated TOTP-based login, and email notifications. The database grows approximately 6GB per day with full Nifty 500 and options coverage.

## Tech Stack

- Python 3
- Zerodha KiteConnect SDK (websocket + REST API)
- MySQL/MariaDB for tick storage
- Selenium + Chrome for automated Zerodha login
- Pandas, NumPy for data handling
- Gmail (smtplib) for notifications
- pyotp for TOTP generation

## Setup

```bash
git clone https://github.com/rthennan/ZerodhaWebsocket.git
cd ZerodhaWebsocket

# Update dasConfig.json with Zerodha, MySQL, and Gmail credentials
# See dasConfig.json for all required fields

pip install -r requirements.txt
```

## Usage

```bash
# Run the full daily pipeline (login -> subscribe -> tick -> backup)
python DAS_main.py

# Manual access token if automation fails
python manualAccessTokenReq.py

# Update Nifty 500 instrument list
python nifty500Updater.py

# Create lookup tables
python lookupTablesCreator.py

# Check/download Zerodha instrument snapshot
python zerodhaInstrumentSnapshot.py
```

## Project Structure

- `DAS_main.py` - Main orchestrator (runs all steps in sequence)
- `DAS_Ticker.py` - Websocket tick receiver and MySQL storage
- `DAS_dailyBackup.py` - End-of-day backup: splits daily table into individual instrument tables
- `accessTokenReq.py` - Automated Zerodha login via Selenium
- `manualAccessTokenReq.py` - Manual fallback for access token
- `nifty500Updater.py` - Dynamic Nifty 500 list maintenance
- `lookupTablesCreator.py` - Creates instrument token lists and lookup dictionaries
- `tradeHolidayCheck.py` - Checks NSE trading holidays
- `dasConfig.json` - All credentials and configuration
- `DAS_gmailer.py` / `DAS_attachmentMailer.py` - Email notifications
- `DAS_errorLogger.py` - Centralized error logging
- `getExpiryPrefix/` - Expiry prefix generator for backtesting
