# ADIDAS SALES WEB APP-PROJECT
That's a great idea\! A detailed and visually appealing `README.md` is essential for any GitHub project.

Since GitHub READMEs use Markdown and not direct CSS, we'll use **emojis**, **code highlighting**, and **Markdown badges** to add "color" and visual structure.

Here is the comprehensive `README.md` content for your **Interactive Adidas Sales Dashboard** project.

-----

# 📈 Interactive Adidas Sales Dashboard

### Live Analysis and Visualization of Retail Data

This project delivers a dynamic, browser-based dashboard built using **Streamlit** to analyze and visualize sales data from an Excel file. It provides immediate insights into retailer performance, geographical sales distribution, and key sales trends over time.

-----

## ✨ Features at a Glance

| Feature | Description | Tools Used |
| :--- | :--- | :--- |
| 📊 **Interactive Visuals** | Dynamic, hoverable charts for drill-down analysis (Plotly). | `plotly.express`, `plotly.graph_objects` |
| 🗺️ **Geographic Insights** | Treemap visualization showing sales breakdown by Region and City. | `plotly.express` |
| 💾 **Data Export** | Built-in Streamlit download buttons for raw data and aggregated results. | `st.download_button` |
| ⚙️ **Robust Data Handling** | Reads proprietary `.xlsx` files and handles time-series analysis. | `pandas` |

-----

## 🛠️ Project Setup

### Prerequisites

To run this application locally, you must have Python installed, along with the required libraries.

```bash
# Ensure you are inside your virtual environment (.venv)
pip install streamlit pandas plotly openpyxl Pillow
```

### Running the App Locally

1.  **Ensure File Structure:** Place the main script and data files in the same directory:
    ```
    Streamlit_dev/
    ├── main.py
    ├── Adidas.xlsx          <-- The data file
    └── adidas-logo.jpg      <-- The image file
    ```
2.  **Execute:** Navigate to the `Streamlit_dev` folder in your terminal and run the Streamlit command (ensure your virtual environment is active):
    ```bash
    streamlit run main.py
    ```
3.  **Access:** The application will automatically open in your default web browser (usually at `http://localhost:8501`).

-----

## 💻 Core Code Logic (with Comments)

The following commented snippets highlight the core functionality responsible for data reading, visualization, and layout.

### Data Loading (Fixing Absolute Paths)

To prevent `FileNotFoundError`, the script uses the absolute path for both the data and the logo image.

```python
# Import necessary libraries
import streamlit as st
import pandas as pd
from PIL import Image

# Use the ABSOLUTE PATH to prevent FileNotFoundError, which was a common issue.
# Replace this path with your own local path if running outside the 'Streamlit_dev' folder.
EXCEL_PATH = "/Users/anthonyanthony/Desktop/WHITEHOUSE/Streamlit_dev/Adidas.xlsx"
IMAGE_PATH = "/Users/anthonyanthony/Desktop/WHITEHOUSE/Streamlit_dev/adidas-logo.jpg"

# Reading the data from excel file
df = pd.read_excel(EXCEL_PATH)

# Load logo image using the absolute path
try:
    image = Image.open(IMAGE_PATH)
except FileNotFoundError:
    st.error("Logo image not found! Please check the IMAGE_PATH.")
```

### Layout and Header Customization

This section uses Streamlit columns for a visually appealing, professional header structure.

```python
# Set the page configuration to a wide layout
st.set_page_config(layout="wide")

# Custom HTML for a centralized and styled title
html_title = """
    <style>
    .title-test {
    font-weight:bold;
    padding:5px;
    border-radius:6px;
    }
    </style>
    <center><h1 class="title-test">Adidas Interactive Sales Dashboard</h1></center>"""

# Use columns for logo placement and title display
col1, col2 = st.columns([0.1,0.9])
with col1:
    st.image(image, width=100) # Display the logo
with col2:
    st.markdown(html_title, unsafe_allow_html=True) # Display the styled title
```

### Dual-Axis Chart for Sales & Units Sold

This uses **Plotly Graph Objects** for advanced chart configuration, specifically to overlay a line chart (Units Sold) on a bar chart (Total Sales).

```python
# Group data by State
result1 = df.groupby(by="State")[["TotalSales","UnitsSold"]].sum().reset_index()

# Initialize the figure
fig3 = go.Figure()

# 1. Add Bar Chart for Total Sales (Primary Y-axis)
fig3.add_trace(go.Bar(x=result1["State"], y=result1["TotalSales"], name="Total Sales"))

# 2. Add Scatter/Line Chart for Units Sold (Secondary Y-axis: yaxis="y2")
fig3.add_trace(go.Scatter(x=result1["State"], y=result1["UnitsSold"], mode="lines",
                          name="Units Sold", yaxis="y2"))

# Update layout to define the two axes
fig3.update_layout(
    title="Total Sales and Units Sold by State",
    xaxis=dict(title="State"),
    yaxis=dict(title="Total Sales", showgrid=False),
    yaxis2=dict(title="Units Sold", overlaying="y", side="right"), # Define secondary axis
    template="gridon",
    legend=dict(x=1, y=1.1)
)

# Display the chart in the app
st.plotly_chart(fig3, use_container_width=True)
```

-----

## 🚀 Get Involved

Feel free to fork this repository, explore the data, and contribute improvements\!

  * **Bugs:** If you find any issues, please open an issue in the repository.
  * **Enhancements:** Suggestions for new visualizations or features are always welcome.
