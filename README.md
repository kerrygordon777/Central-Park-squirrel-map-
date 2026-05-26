# Central-Park-squirrel-map-
This project explores the 2018 Central Park Squirrel Census from NYC Open Data. The goal is to find the hectare with the friendliest squirrels — defined as the highest ratio of squirrels approaching humans.
# 🐿️ Central Park Squirrel Census — Behavior Analysis

**Which part of Central Park has the friendliest squirrels?**

This project explores the [2018 Central Park Squirrel Census](https://data.cityofnewyork.us/Environment/2018-Central-Park-Squirrel-Census-Squirrel-Data/vfnx-vebw/about_data) dataset published by NYC Open Data. Using Python, I analyzed squirrel behavior across 350 hectares of Central Park to find which area had the highest rate of squirrels approaching humans.

---

## 📊 Dataset

- **Source:** NYC Open Data — 2018 Central Park Squirrel Census
- **Records:** 3,023 squirrel sightings
- **Key columns used:** `Approaches`, `Runs from`, `Indifferent`, `Hectare`, `X`, `Y`

---

## 🔍 Approach

1. Loaded and inspected the dataset
2. Isolated the three behavior columns: `Approaches`, `Runs from`, and `Indifferent`
3. Grouped sightings by hectare and calculated a **friendliness ratio** — the proportion of squirrels that approached humans out of all observed squirrels in that hectare
4. Filtered for hectares with at least 10 observations to ensure reliable results
5. Visualized the friendliest and least friendly hectares using bar charts
6. Built an interactive map of Central Park using Folium, with each hectare colored by friendliness ratio

---

## 📈 Key Finding

**Hectare 07H is the friendliest spot in Central Park.** Squirrels in this area approached humans at a higher rate than any other hectare with a reliable sample size.

The interactive map (`squirrel_map.html`) shows the full picture — bright blue dots indicate the friendliest hectares, while transparent dots indicate areas where squirrels tend to run from humans.

---

## 🗺️ Interactive Map

Open `squirrel_map.html` in any browser to explore the map. Click any dot to see the hectare ID and its friendliness ratio.

---

## 🛠️ Tools Used

- Python 3
- Pandas
- Matplotlib
- Folium
- Jupyter Notebook

---

## 📁 Files

| File | Description |
|------|-------------|
| `EDA_Project_1.ipynb` | Main Jupyter notebook with all analysis |
| `2018_squirrel_Data.csv` | Raw dataset from NYC Open Data |
| `squirrel_map.html` | Interactive Central Park friendliness map |

---

## 🚀 How to Run

1. Clone this repository
2. Install dependencies: `pip install pandas matplotlib folium`
3. Open `EDA_Project_1.ipynb` in Jupyter Notebook
4. Run all cells in order
