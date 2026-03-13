# 🔬 Flu vs. COVID-19 Trends (2020–2024) in New Jersey

> Comparative time-series analysis of influenza vs. COVID-19 incidence across New Jersey (2020–2024), leveraging Python-powered ETL pipelines, interactive dashboards, and rigorous statistical modeling.

---

## 📅 Project Date  
**Completed:** December 15, 2024

---

## 🗂️ Datasets

1. **Johns Hopkins CSSE**  
   `time_series_covid19_confirmed_US.csv`  
   – Daily confirmed COVID-19 cases across U.S. counties.  
2. **WHO NREVSS Clinical Labs**  
   `WHO_NREVSS_Clinical_Labs.csv`  
   – Weekly influenza test results reported by clinical laboratories.

---

## 🚀 Features & Highlights

- **🌟 Data Cleaning & Harmonization**  
  – Automated scripts to merge disparate date formats, fill missing values, and normalize county-level counts.  
- **📊 Aggregation & Comparison**  
  – Weekly rolling averages of flu vs. COVID-19 cases, per-capita normalization, and seasonality overlays.  
- **🔍 Interactive Visualization**  
  – Dynamic dashboards built with **Hvplot** & **Panel** for drill-down by county, timeframe, and age cohort.  
- **📈 Statistical Modeling**  
  – Trend decomposition (STL), correlation analysis, and basic forecasting to uncover leading indicators.  
- **⚙️ Reproducibility**  
  – Parameterized notebook cells and configuration file for easy reruns on updated data.

---

## 🛠️ Installation & Requirements

Create a clean Python environment (recommended via `venv` or `conda`) and install exact dependencies:

```bash
pip install pandas==2.0.3 \
            numpy==1.25.2 \
            matplotlib==3.7.2 \
            seaborn==0.12.2 \
            hvplot==0.9.0 \
            panel==1.6.1

# Clone the repository
git clone https://github.com/Elgen69/Flu-vs-COVID-19-Trends-2020-2024-in-New-Jersey.git
cd Flu-vs-COVID-19-Trends-2020-2024-in-New-Jersey

# Launch JupyterLab or Notebook
jupyter lab

# Open & run notebook.ipynb
# Make sure the CSVs (cleaned_flu_data.csv, merged_covid_flu_data.csv) are in the same folder.
# Interact with the Panel dashboard by executing the final cell.

📝 License & Attribution
© 2024 Elgen Mar Arinasa. All rights reserved.
This notebook is released for educational and research purposes only. Proper attribution to the original author is required for any redistribution or derivative works. Unauthorized claiming of this work is strictly prohibited.

“Data tells the story—our job is to listen carefully and let the insights emerge.”
— Elgen Mar Arinasa
