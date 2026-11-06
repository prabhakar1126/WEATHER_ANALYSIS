[README.md](https://github.com/user-attachments/files/34484772/README.md)
# Weather Data Analysis System

A beginner-friendly web application built with **Python, Flask, MySQL, Pandas, NumPy and Matplotlib**. It allows users to upload historical weather data in CSV format, validate and store the records, view summaries, and explore the results through charts.

> **Note:** This project analyses historical weather observations. It does not predict or forecast future weather.

## Features

- **Dashboard:** View the total number of records, average temperature, average humidity, total rainfall and available locations.
- **CSV upload:** Import weather observations from a CSV file.
- **Data validation:** Check required columns, dates, missing values and measurement ranges. Invalid rows are reported with their CSV row numbers and rejection reasons.
- **Duplicate protection:** Skip records that have the same observation date and location as an existing record, including duplicates within the uploaded file.
- **Weather analysis:** Calculate overall and monthly statistics using Pandas and NumPy.
- **Charts:** Display a temperature trend, monthly rainfall, monthly average humidity, and temperature-versus-rainfall chart.
- **Record management:** View saved records, delete an individual record, or delete all records after confirmation.
- **JSON endpoints:** Check application/database health and retrieve a summary through the API.

## Technology stack

| Area | Technology |
|---|---|
| Backend | Python, Flask |
| Database | MySQL |
| Data processing | Pandas, NumPy |
| Charts | Matplotlib |
| Frontend | HTML, CSS, JavaScript, Jinja templates |
| Testing | pytest |

## Project structure

```text
Weather_Data_Analysis_System/
├── analysis/
│   ├── __init__.py
│   └── weather_analysis.py   # CSV validation, statistics and chart generation
├── database/
│   ├── schema.sql            # Database and table creation
│   └── upgrade_duplicate_protection.sql
├── static/
│   ├── css/style.css
│   └── charts/               # Generated chart images
├── templates/                # Flask HTML pages
├── tests/
│   └── test_analysis.py
├── uploads/                  # Uploaded CSV files
├── .env.example              # Example environment configuration
├── app.py                    # Flask application entry point
├── requirements.txt
├── sample_weather.csv        # Example dataset
├── DATA_DICTIONARY.md
├── PROJECT_SYNOPSIS.md
├── TEST_PLAN.md
└── README.md
```

## Requirements

- Python 3.10 or newer
- MySQL Server 8.x (or a compatible version)
- pip
- A web browser
- MySQL Workbench or MySQL command-line client (recommended for database setup)

## Installation and setup

The following commands are for **Windows PowerShell**. Run them from the project directory.

### 1. Create and activate a virtual environment

```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
```

If PowerShell blocks script activation, run the following command in that terminal and activate the environment again:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.venv\Scripts\Activate.ps1
```

### 2. Install dependencies

```powershell
python -m pip install -r requirements.txt
```

### 3. Create the MySQL database

1. Start your MySQL server.
2. Open MySQL Workbench or the MySQL command line.
3. Run the complete `database/schema.sql` script.
4. The script creates the `weather_analysis` database and the `weather_records` table.

### 4. Configure database credentials

Copy `.env.example` to a new file named `.env` in the project root. Update the values to match your local MySQL configuration:

```env
SECRET_KEY=replace-with-a-random-secret
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_mysql_password
DB_NAME=weather_analysis
```

Keep your real `.env` file private. Do not upload database passwords or secret keys to GitHub. The application also supports the equivalent `MYSQL_HOST`, `MYSQL_PORT`, `MYSQL_USER`, `MYSQL_PASSWORD` and `MYSQL_DATABASE` environment variable names.

### 5. Start the application

```powershell
python app.py
```

Open this address in your browser:

```text
http://127.0.0.1:5000
```

## Try the project with the sample dataset

1. Start the application and open the dashboard.
2. Select **Upload dataset**.
3. Choose `sample_weather.csv` from the project folder.
4. Review the validation report and import result.
5. Open **Analysis & charts** to view the summary and charts.
6. Open **Weather records** to view saved observations.
7. Try deleting one record. You can also use **Delete all records** to clear the database table after confirming the action.

Deleting records removes database rows; it does not delete the original CSV file from your computer or the `uploads/` folder.

## CSV file format

The uploaded CSV must contain these column names. Column-name matching is case-insensitive.

| Column | Description | Validation |
|---|---|---|
| `date` | Date of observation | Must be a valid date; `YYYY-MM-DD` is recommended |
| `location` | City or weather-station name | Must not be empty |
| `temperature` | Temperature | Degrees Celsius; -90 to 65 |
| `humidity` | Relative humidity | Percentage; 0 to 100 |
| `rainfall` | Rainfall amount | Millimetres; 0 or greater |

Example:

```csv
date,location,temperature,humidity,rainfall
2026-01-01,Visakhapatnam,28.5,72,2.4
2026-01-02,Visakhapatnam,29.1,69,0
2026-01-03,Visakhapatnam,27.8,81,8.2
```

Rows with missing or invalid required values are rejected and listed in the data-quality report. The application does not automatically fill missing values. A record is considered a duplicate when its observation date and normalized location match an existing record. The same date at a different location is allowed.

Use public, synthetic, anonymized or otherwise approved datasets. Uploaded CSV files are saved locally in `uploads/`.

## Analysis and charts

The application calculates overall statistics and groups records by calendar month. The analysis includes:

- Average, minimum and maximum temperature
- Average humidity
- Total rainfall
- Monthly average, minimum and maximum temperature
- Monthly average humidity and total rainfall

The charts generated are:

1. Daily temperature trend
2. Monthly rainfall totals
3. Monthly average humidity
4. Temperature versus rainfall scatter plot

Temperature is measured in **°C**, humidity in **%**, and rainfall in **mm**.

## Available routes

| Method | Route | Description |
|---|---|---|
| `GET` | `/` | Dashboard |
| `GET`, `POST` | `/upload` | Upload, validate and import a CSV file |
| `GET` | `/analysis` | Summary tables and charts |
| `GET` | `/records` | View the latest 500 stored records |
| `POST` | `/records/<record_id>/delete` | Delete one weather record |
| `POST` | `/records/delete-all` | Delete all weather records |
| `GET` | `/api/health` | Application and database health check |
| `GET` | `/api/summary` | Retrieve a JSON weather summary |

## Run tests

With the virtual environment activated, run:

```powershell
pytest -q
```

The included tests cover empty summaries, summary calculations and CSV validation. For a complete manual check, also test a valid CSV, missing columns, invalid dates, missing measurements, out-of-range values, duplicate records, an empty database and an unavailable MySQL server.

## Database overview

The main table is `weather_records`:

- `record_id` — auto-increment primary key
- `observation_date` — date of the observation
- `location` — city or station name
- `temperature` — temperature in °C
- `humidity` — relative humidity in %
- `rainfall` — rainfall in mm
- `created_at` — database insertion timestamp

See [`DATA_DICTIONARY.md`](DATA_DICTIONARY.md) for field details and validation rules.

## Limitations and future improvements

- The current version does not include user login or role-based access.
- It analyses uploaded historical observations and does not provide weather forecasting.
- Results depend on the accuracy and coverage of the uploaded dataset.
- Future versions could add location filters, exportable reports, interactive charts, user authentication and forecasting using a suitable weather dataset.

## Contributing

Suggestions and improvements are welcome. For a change, create a branch, make the update, run the tests, and submit a pull request with a clear description of the change.

## License

No license file is currently included in this repository. Add a `LICENSE` file before describing the project as open source or allowing reuse under specific terms.
