# Sales Dashboard and Forecast

A simple web tool that turns a company's past sales data into a **sales forecast** and **plain-language business insights**. Built for the *Code & Chaos* hackathon, problem statement **PS-03: Sales Forecasting & Business Insights**.

> Team: **[team name]** | Members: **[names]**

## The problem

A retail company has years of sales data (product, quantity, price, date, region) but no easy way to see what it shows or what next month might look like. Sales managers are not programmers, so the tool has to be usable without training.

## What it does

- Upload a **CSV or Excel** file (or click **Use sample data** to try it).
- Detects the date, sales, region, category and product columns automatically (you can change them).
- **Forecasts the next 3, 6 or 12 months** with a shaded "likely range".
- Compares sales **by region, by category, and the top 10 products**.
- Writes **business insights** with suggested actions (best and weakest region, busiest month, and so on).
- Runs an **accuracy check**: hides the last 3 months, forecasts them, and reports the average error.
- Can filter everything by one region or category.
- Works on laptop and phone, in light or dark mode.

## Screenshots (made with sample data)

| Upload and controls | Forecast |
|---|---|
| ![Controls](1-upload-and-controls.png) | ![Forecast](2-forecast-chart.png) |

| Sales comparison | Insights |
|---|---|
| ![Comparison](3-sales-comparison.png) | ![Insights](4-insights.png) |

Full page: [full-page.png](full-page.png)

## How to run

No installation is needed.

1. Download or clone this repository.
2. Open `index.html` in any modern browser (Chrome, Edge, Firefox, Safari).
3. Click **Use sample data**, or upload your own file. A test file is included at `sample_sales.csv`.

An internet connection is only needed to read **Excel** files (the SheetJS library loads from a CDN). CSV files and the sample data work offline.

**Optional, publish it online with GitHub Pages:** repository Settings, then Pages, choose the `main` branch and the root folder. GitHub gives you a public link.

## Your data file

One row per sale (or per order line), with a header row. Column names can differ; the tool guesses them.

| Needed | Example column names |
|---|---|
| Date (required) | `Order Date`, `Date` |
| Sales amount (required) | `Sales`, `Revenue`, `Amount` |
| Region (optional) | `Region` |
| Category (optional) | `Category` |
| Product (optional) | `Product Name`, `Product` |

At least **6 months** of data are needed for a forecast, and **24 months** or more for the seasonal adjustment. Free test data: the "Superstore Sales" dataset on Kaggle works directly.

## How the forecast works

1. Sales are added up **month by month**.
2. A **straight trend line** is fitted through the months (least-squares linear regression). This shows whether sales are rising or falling.
3. With 24 or more months of data, a **seasonal index** is learned for each calendar month (for example December is usually higher). The trend is multiplied by it.
4. The **likely range** is the forecast plus or minus 1.28 times the typical past error, which covers about 80% of typical cases.
5. The **accuracy check** (backtest) hides the last 3 months, forecasts them, and compares with the real values.

This is a basic, explainable method. It cannot predict sudden events such as a new competitor or a supply problem.

## Technology

| Part | Technology |
|---|---|
| Page and layout | HTML5 |
| Design (light and dark mode, responsive) | CSS3 with variables |
| Logic | Plain JavaScript (no framework, no build step) |
| Excel reading | [SheetJS](https://sheetjs.com/) (open source, loaded from cdnjs) |
| Chart | SVG drawn by our own code |
| Forecast | Linear regression plus seasonal index |
| Privacy | Everything runs in the browser; files are never uploaded to a server |

## Project files

- `index.html`: page structure
- `style.css`: all styling, light and dark mode
- `utils.js`: helpers (CSV reading, date and number conversion)
- `data.js`: loaded rows, monthly totals, totals per region and category, sample data
- `forecast.js`: forecast method and accuracy check
- `charts.js`: forecast chart (SVG) and comparison bars
- `insights.js`: business insights text
- `app.js`: connects the page (file loading, controls, main run)
- `sample_sales.csv`: test file with made-up sales
- `*.png`: screenshots used in this README

## Limitations

- Forecasts use past patterns only and can be wrong, which is why a range and an accuracy check are shown.
- Needs at least 6 months of data (24 for seasonality).
- Sample data is made up and only meant for demonstration.

## Future scope

Forecast per product, export results to Excel, low-stock alerts, and more advanced machine learning models.

## Note on tools

An AI assistant (Claude) was used to help write and refine the code. The team tested the tool with the dataset and can explain every step of the method above.
