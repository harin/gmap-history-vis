# Visualizing Google Map History

I was wondering where in NYC have I not been to yet. Since I am using have Google Map Timeline feature turned on, I decided it probably is not too hard to make that useful and find areas I have been neglecting. Turns out, all it takes is a few lines in python.

Here's the TL;DR code

```python
import json
import pandas as pd
import folium
from folium import plugins

with open('Location History.json') as f:
    data = json.load(f)
df = pd.DataFrame(data['locations'])

def to_coord(df):
    return (df[['latitudeE7', 'longitudeE7']]/1e7).as_matrix()

def heatmap_coords(coords):
    m = folium.Map(list(coords[0]), zoom_start=13)
    m.add_children(plugins.HeatMap(coords, radius=20))
    return m

# limit to 50k otherwise map might not show properly
heatmap_coords(coords[:50000])

```

Here's the result

![My Location History](https://github.com/harin/gmap-history-vis/raw/master/location-history.png)
