1. Import Required Libraries
============================================
import matplotlib.pyplot as plt
import pandas as pd
import plotly.express as px

2. Load and Clean First Dataset (df1)
============================================
# Load dataset
df1 = pd.read_csv(r"C:\Users\DELL\Documents\Projects\data\brasil_real-estate-2.csv")
df1.head()
df1.info()

# Check shape and info
df1.shape
df1.info()

# Drop missing values
df1.dropna(inplace=True)
df1.head()

# Split 'lat-lon' column into separate 'lat' and 'lon' columns
df1[["lat", "lon"]] = df1["lat-lon"].str.split(",", expand=True).astype(float)
df1.head()
df1.info()

# Extract 'state' from 'place_with_parent_names' column
df1["state"] = df1["place_with_parent_names"].str.split("|", expand=True)[2]
df1.head()

# Convert 'price_usd' from string to float
df1["price_usd"] = df1["price_usd"].str.replace("$", "", regex=False).str.replace(",", "").astype(float)
df1.info()
df1.head()

# Drop unnecessary columns
df1.drop(columns=["lat-lon", "place_with_parent_names"], inplace=True)
df1.head()

3. Load and Clean Second Dataset (df2)
============================================
# Load dataset
df2 = pd.read_csv(r"C:\Users\DELL\Documents\Projects\data\brasil_real-estate-2.csv")
df2.head()
df2.info()

# Convert 'price_brl' to 'price_usd' using exchange rate
df2["price_usd"] = (df2["price_brl"] / 3.19).round(2)
df2.head()

# Drop missing values and remove 'price_brl' column
df2.dropna(inplace=True)
df2.drop(columns=["price_brl"], inplace=True)
df2.head()

4. Combine Both Datasets
============================================
df = pd.concat([df1, df2])
df.head()

5. Visualize Property Locations on Map
============================================
fig = px.scatter_mapbox(
    df,
    lat="lat",
    lon="lon",
    center={"lat": -14.2, "lon": -51.9},  # Center map on Brazil
    width=600,
    height=600,
    hover_data=["price_usd"],  # Show price on hover
)

# Use Open Street Map style
fig.update_layout(mapbox_style="open-street-map")
fig.show()

6. Summary Statistics
============================================
summary_stats = df[["area_m2", "price_usd"]].describe()
summary_stats

7. Distribution of Home Prices (Histogram)
============================================
fig, ax = plt.subplots()
ax.hist(df["price_usd"].iloc[:20000])
ax.set_xlabel("Price [USD]")
ax.set_ylabel("Frequency")
ax.set_title("Distribution of Home Prices")

8. Distribution of Home Sizes (Boxplot)
============================================
fig, ax = plt.subplots()
ax.boxplot(df["area_m2"], vert=False)
ax.set_xlabel("Area [sq meters]")
ax.set_title("Distribution of Home Sizes")

9. Mean Price by Region (Bar Chart)
============================================
mean_price_by_region = df.groupby("region")["price_usd"].mean()

fig, ax = plt.subplots()
mean_price_by_region.plot(
    kind="bar",
    xlabel="Region",
    ylabel="Mean Price [USD]",
    title="Mean Home Price by Region"
)

10. Analyze South Region
============================================
# Filter for South region
df_south = df[df["region"] == "South"]
df_south.head()

# Count homes by state
homes_by_state = df_south["state"].value_counts()
homes_by_state

# Find state with most listings
most_common_state = df_south["state"].value_counts().idxmax()
df_south_rgs = df_south[df_south["state"] == most_common_state]

11. Price vs. Area in Most Popular South State (Scatter Plot)
============================================
fig, ax = plt.subplots()
ax.scatter(x=df_south_rgs["area_m2"], y=df_south_rgs["price_usd"])
ax.set_xlabel("Area [sq meters]")
ax.set_ylabel("Price [USD]")
ax.set_title("Rio Grande do Sul: Price vs. Area")
