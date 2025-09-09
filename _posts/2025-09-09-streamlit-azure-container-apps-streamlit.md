---
title: "Deploy Streamlit to Azure Container Apps with IAC and CICD - Creating a Streamlit Application"
layout: post
date: "2025-09-09"
permalink: posts/streamlit-azure-container-apps-streamlit/
categories: [Streamlit to Azure Container Apps with IAC and CICD]
tags:
  [
    streamlit,
    azure,
    azure-container-apps,
    terraform,
    github-actions,
    cicd,
    iac,
    cloud,
    docker,
  ]
# img_path: /assets/images/position-images-next-to-div/
---

In this post I will go over the Python code for the a simple 4 page application that demonstrates some of the key capabilities of streamlit.

This post is part 2 of a [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) of posts where I will write about my solution for getting a streamlit application deployed to Azure Container Apps with IAC and CICD.

The code repository for this tutorial is available on my GitHub repo [streamlit-azure-container-app](https://github.com/jarrodlilkendey/streamlit-azure-container-app).

If you would like to check out the application before continuining with the post, take a look over at [https://streamlit.jarrodlilkendey.com](https://streamlit.jarrodlilkendey.com).

## Creating a Streamlit Application

The streamlit application that I am creating is fairly simple. It is a 4 page web application with a navigation side bar. The pages are described in more detail in the sections below.

### Streamlit Dependencies

There are two dependencies required to support this streamlit application, they are defined in the requirements.txt file.

{: file='requirements.txt'}

```text
streamlit==1.48.1
streamlit[charts]
```

Before you download the dependencies, the nest practice is to first set up a Python virtual environment.

This can be done with the commands below.

```
# create virtual environment
python -m venv .venv

# Windows command prompt
.venv\Scripts\activate

# Windows PowerShell

.venv\Scripts\Activate.ps1

# macOS and Linux

source .venv/bin/activate
```

From within the virtual environment, to download the dependencies using pip3 and the requirements.txt file use the following command.

```
pip3 install -r requirements.txt
```

### The Home Page

The Home page provides links off to the 3 other pages in the streamlit application.

{: file='home.py'}

```python
import streamlit as st

st.title("Home")

st.page_link("dataframes.py", label="Pandas dataframes with streamlit", icon="👨‍💻")
st.page_link("inputs.py", label="Inputs supported by streamlit", icon="⌨️")
st.page_link("visuals.py", label="Streamlit visualisations", icon="📊")
```

All of these pages are also linked in the navigation side bar along with the Home page itself.

### The Dataframes Page

The dataframes page demonstrates the capabilities of using pandas dataframes with streamlit for CSV that are already uploaded or selected from a CSV file hosted within the streamlit application.

{: file='dataframes.py'}

```python
import streamlit as st
import pandas as pd
import os

st.title("Pandas dataframes with streamlit")

st.write("Load a CSV file into a pandas dataframe to display with streamlit")

file_option = st.radio(
    "CSV file option",
    ["Use a sample file", "Upload my CSV"],
)

def render_dataframe_with_statistics(file):
    df = pd.read_csv(file)

    st.write("### Data Preview")
    st.dataframe(df)

    st.write("### Summary Statistics")
    st.write(df.describe())

if file_option == "Upload my CSV":
    uploaded_file = st.file_uploader("Choose a file")
    if uploaded_file is not None:
        render_dataframe_with_statistics(uploaded_file)

if file_option == "Use a sample file":
    sample_csv_file_path = st.selectbox(
        "Sample CSV file",
        ("data/csv/action_movies.csv", "data/csv/south_park_characters.csv", "data/csv/sample_weather_data.csv"),
        index=None,
        placeholder="Select sample CSV file..."
    )

    st.write("You selected:", sample_csv_file_path)

    if sample_csv_file_path is not None:
        if os.path.exists(sample_csv_file_path):
            render_dataframe_with_statistics(sample_csv_file_path)
        else:
            st.error(f"File not found at {sample_csv_file_path}")
```

### The Inputs Page

The inputs page demostrates a sub selection of the input elements supported by streamlit.

{: file='inputs.py'}

```python
import streamlit as st
import datetime

st.title("Inputs supported by streamlit")

st.header("button", divider=True)

left, middle, right = st.columns(3)
if left.button("Plain button", width="stretch"):
    left.markdown("You clicked the plain button.")
if middle.button("Emoji button", icon="😃", width="stretch"):
    middle.markdown("You clicked the emoji button.")
if right.button("Material button", icon=":material/mood:", width="stretch"):
    right.markdown("You clicked the Material button.")

st.header("checkbox", divider=True)

agree = st.checkbox("I agree")
if agree:
    st.write("Great!")

st.header("radio", divider=True)

genre = st.radio(
    "What's your favorite movie genre",
    [":rainbow[Comedy]", "***Drama***", "Documentary :movie_camera:"],
    index=None,
)

st.write("You selected:", genre)

st.header("selectbox", divider=True)

option = st.selectbox(
    "How would you like to be contacted?",
    ("Email", "Home phone", "Mobile phone"),
)

st.write("You selected:", option)

st.header("number_input", divider=True)

number = st.number_input("Insert a number")
st.write("The current number is ", number)

st.header("date_input", divider=True)

d = st.date_input("When's your birthday", datetime.date(2019, 7, 6))
st.write("Your birthday is:", d)


st.header("time_input", divider=True)

t = st.time_input("Set an alarm for", datetime.time(8, 45))
st.write("Alarm is set for", t)

st.header("text_input", divider=True)

title = st.text_input("Movie title", "Life of Brian")
st.write("The current movie title is", title)
```

### The Visuals Page

The visuals page demonstrates a selection of visualisations within streamlit using a variety of python visualisation libraries

{: file='visuals.py'}

```python
import streamlit as st
import altair as alt
import pandas as pd
import streamlit as st
from numpy.random import default_rng as rng
import plotly.express as px
import pydeck
import matplotlib.pyplot as plt

st.title("Visualisations")

st.header("altair_chart", divider=True)

df = pd.DataFrame(rng(0).standard_normal((60, 3)), columns=["a", "b", "c"])

chart = (
    alt.Chart(df)
    .mark_circle()
    .encode(x="a", y="b", size="c", color="c", tooltip=["a", "b", "c"])
)

st.altair_chart(chart)

st.header("graphviz_chart", divider=True)

st.graphviz_chart('''
    digraph {
        run -> intr
        intr -> runbl
        runbl -> run
        run -> kernel
        kernel -> zombie
        kernel -> sleep
        kernel -> runmem
        sleep -> swap
        swap -> runswap
        runswap -> new
        runswap -> runmem
        new -> runmem
        sleep -> runmem
    }
''')

st.header("plotly_chart", divider=True)

plotly_df = px.data.iris()
fig = px.scatter(plotly_df, x="sepal_width", y="sepal_length")

event = st.plotly_chart(fig, key="iris", on_select="rerun")

st.header("pydeck_chart", divider=True)

capitals = pd.read_csv(
    "data/csv/capitals.csv",
    header=0,
    names=[
        "Capital",
        "State",
        "Abbreviation",
        "Latitude",
        "Longitude",
        "Population",
    ],
)
capitals["size"] = capitals.Population / 10

point_layer = pydeck.Layer(
    "ScatterplotLayer",
    data=capitals,
    id="capital-cities",
    get_position=["Longitude", "Latitude"],
    get_color="[255, 75, 75]",
    pickable=True,
    auto_highlight=True,
    get_radius="size",
)

view_state = pydeck.ViewState(
    latitude=40, longitude=-117, controller=True, zoom=2.4, pitch=30
)

chart = pydeck.Deck(
    point_layer,
    initial_view_state=view_state,
    tooltip={"text": "{Capital}, {Abbreviation}\nPopulation: {Population}"},
)

event = st.pydeck_chart(chart, on_select="rerun", selection_mode="multi-object")

st.header("pyplot", divider=True)

arr = rng(0).normal(1, 1, size=100)
fig, ax = plt.subplots()
ax.hist(arr, bins=20)

st.pyplot(fig)

st.header("vega_lite_chart", divider=True)

st.vega_lite_chart(
    df,
    {
        "mark": {"type": "circle", "tooltip": True},
        "encoding": {
            "x": {"field": "a", "type": "quantitative"},
            "y": {"field": "b", "type": "quantitative"},
            "size": {"field": "c", "type": "quantitative"},
            "color": {"field": "c", "type": "quantitative"},
        },
    },
)
```

### The Application

The streamlit app.py file containing the main file for running the streamlit application, it defines adds the 4 pages to a navigation bar then runs the application.

{: file='app.py'}

```python
import streamlit as st

# Define the pages
home_page = st.Page("home.py", title="Home Page", icon="🏠")
dataframes_page = st.Page("dataframes.py", title="Dataframes", icon="👨‍💻")
inputs_page = st.Page("inputs.py", title="Inputs", icon="⌨️")
visuals_page = st.Page("visuals.py", title="Visualisations", icon="📊")

# Set up navigation
pg = st.navigation([home_page, dataframes_page, inputs_page, visuals_page])

# Run the selected page
pg.run()
```

You can now run the streamlit app using the following command.

```
streamlit run app.py
```

The application should be accessible in the browser on localhost on port 8501.

## Related Posts

This post is part 2 of a [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) of posts where I will write about my solution for getting a streamlit application deployed to Azure Container Apps with IAC and CICD.

See all of the posts included in this [series](/categories/streamlit-to-azure-container-apps-with-iac-and-cicd/) below.

{% assign category = "Streamlit to Azure Container Apps with IAC and CICD" %}

<ul>
  {% for post in site.categories[category] reversed %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
