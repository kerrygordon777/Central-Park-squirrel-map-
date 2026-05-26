# 2018 Central Park Squirrel Census — Behavior Analysis

## Overview
This project explores the 2018 Central Park Squirrel Census dataset from NYC Open Data.
The goal is to find the hectare with the friendliest squirrels — defined as the
highest ratio of squirrels approaching humans.

## Dataset
- **Source:** NYC Open Data
- **Records:** 3,023 squirrels
- **Columns:** 30


import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import folium
import matplotlib.colors as mcolors
from IPython.display import display

print("All good!")


df = pd.read_csv("2018_squirrel_Data.csv")
df.head()

df.info()

df.columns[23:29]

df[['Indifferent', 'Runs from', 'Approaches']].head(10)


print("Indifferent:", df['Indifferent'].sum())
print("Runs from:", df['Runs from'].sum())
print("Approaches:", df['Approaches'].sum())


hectare_behavior = df.groupby('Hectare')[['Indifferent', 'Runs from', 'Approaches']].sum()
hectare_behavior.head(10)



hectare_behavior['total'] = (hectare_behavior['Indifferent']
                           + hectare_behavior['Runs from']
                           + hectare_behavior['Approaches'])

hectare_behavior['scared_ratio'] = hectare_behavior['Runs from'] / hectare_behavior['total']
hectare_behavior['friends_ratio'] = hectare_behavior['Approaches'] / hectare_behavior['total']


reliable = hectare_behavior[hectare_behavior['total'] >= 10]

# Preview top friendly hectares
reliable.sort_values('friends_ratio', ascending=False).head(10)



# Friendliest
reliable.sort_values('friends_ratio', ascending=False).head(10)

# Least friendly
reliable.sort_values('friends_ratio', ascending=True).head(10)

top_friendly = reliable.sort_values('friends_ratio', ascending=False).head(5)
top_unfriendly = reliable.sort_values('friends_ratio', ascending=True).head(5)

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(14, 6))

ax1.bar(top_friendly.index, top_friendly['friends_ratio'], color='mediumseagreen')
ax1.set_title('Top 5 Friendliest Hectares')
ax1.set_xlabel('Hectare')
ax1.set_ylabel('Friendly Ratio')
ax1.tick_params(axis='x', rotation=45)

ax2.bar(top_unfriendly.index, top_unfriendly['friends_ratio'], color='tomato')
ax2.set_title('Top 5 Least Friendly Hectares')
ax2.set_xlabel('Hectare')
ax2.set_ylabel('Friendly Ratio')
ax2.tick_params(axis='x', rotation=45)

plt.tight_layout()
plt.show()




approaching = df.groupby('Hectare')['Approaches'].sum()
approaching.sort_values(ascending=False).head(10)


top_approaching = approaching.sort_values(ascending=False).head(5)

plt.figure(figsize=(7, 6))
plt.bar(top_approaching.index, top_approaching.values, color='mediumseagreen')
plt.title('Top 5 Hectares for Squirrel Approaches')
plt.xlabel('Hectare')
plt.ylabel('Number of Approaching Squirrels')
plt.tick_params(axis='x', rotation=45)
plt.tight_layout()
plt.show()



# Build map data
hectare_coords = df.groupby('Hectare')[['X', 'Y']].mean()
map_data = hectare_coords.join(reliable[['friends_ratio']])
map_data = map_data.dropna(subset=['friends_ratio'])

# Color scale from 0 to max
norm = mcolors.Normalize(vmin=0, vmax=map_data['friends_ratio'].max())

# Build map
m = folium.Map(location=[40.7851, -73.9683], zoom_start=14)

for idx, row in map_data.iterrows():
    ratio = norm(row['friends_ratio'])
    color = '#00BFFF'

    folium.CircleMarker(
        location=[row['Y'], row['X']],
        radius=8,
        color=color,
        fill=True,
        fill_color=color,
        fill_opacity=float(ratio),
        opacity=float(ratio),
        popup=f"Hectare: {idx} | Friendly Ratio: {row['friends_ratio']:.2f}"
    ).add_to(m)

m.save('squirrel_map.html')
display(m)
