# WorldGoldDepositsMap_Dr.Mutlu-Zeybek

#Method 1: Using Plotly (Recommended - Most Reliable)
import plotly.graph_objects as go
import pandas as pd

# Create gold deposits dataset
def create_gold_deposits_data():
    deposits = [
        # Major gold deposits worldwide
        {'name': 'Witwatersrand Basin', 'lat': -26.2044, 'lon': 28.0456, 'type': 'Basin', 'size': 100},
        {'name': 'Carlin Trend', 'lat': 40.95, 'lon': -116.13, 'type': 'Carlin-type', 'size': 80},
        {'name': 'Super Pit Kalgoorlie', 'lat': -30.76, 'lon': 121.47, 'type': 'Orogenic', 'size': 70},
        {'name': 'Grasberg', 'lat': -4.06, 'lon': 137.11, 'type': 'Porphyry', 'size': 60},
        {'name': 'Muruntau', 'lat': 41.55, 'lon': 64.61, 'type': 'Orogenic', 'size': 90},
        {'name': 'Pueblo Viejo', 'lat': 18.98, 'lon': -70.15, 'type': 'Epithermal', 'size': 50},
        {'name': 'Boddington', 'lat': -32.80, 'lon': 116.47, 'type': 'Orogenic', 'size': 40},
        {'name': 'Olympic Dam', 'lat': -30.43, 'lon': 136.88, 'type': 'IOCG', 'size': 30},
        {'name': 'Cortez', 'lat': 39.74, 'lon': -116.87, 'type': 'Carlin-type', 'size': 45},
        {'name': 'Lihir', 'lat': -3.12, 'lon': 152.64, 'type': 'Epithermal', 'size': 35},
        # North American deposits
        {'name': 'Homestake', 'lat': 44.35, 'lon': -103.75, 'type': 'Orogenic', 'size': 25},
        {'name': 'Red Lake', 'lat': 51.06, 'lon': -93.83, 'type': 'Orogenic', 'size': 30},
        # African deposits
        {'name': 'Tarkwa', 'lat': 5.30, 'lon': -2.00, 'type': 'Orogenic', 'size': 25},
        {'name': 'Geita', 'lat': -2.87, 'lon': 32.23, 'type': 'Orogenic', 'size': 20},
        # South American deposits
        {'name': 'Yanacocha', 'lat': -6.48, 'lon': -78.35, 'type': 'Epithermal', 'size': 40},
        {'name': 'Veladero', 'lat': -29.33, 'lon': -69.83, 'type': 'Epithermal', 'size': 25},
    ]
    return pd.DataFrame(deposits)

def create_gold_map_plotly():
    df = create_gold_deposits_data()
    
    # Color mapping by deposit type
    color_map = {
        'Orogenic': 'red',
        'Carlin-type': 'blue',
        'Porphyry': 'green',
        'Epithermal': 'orange',
        'Basin': 'purple',
        'IOCG': 'brown'
    }
    
    df['color'] = df['type'].map(color_map)
    
    # Create the map
    fig = go.Figure()
    
    # Add deposits by type
    for deposit_type in df['type'].unique():
        df_subset = df[df['type'] == deposit_type]
        fig.add_trace(go.Scattergeo(
            lon = df_subset['lon'],
            lat = df_subset['lat'],
            text = df_subset['name'] + '<br>Type: ' + df_subset['type'],
            name = deposit_type,
            marker = dict(
                size = df_subset['size'] / 3,
                color = color_map[deposit_type],
                line = dict(width=1, color='black'),
                sizemode = 'area'
            ),
            hovertemplate = '<b>%{text}</b><extra></extra>'
        ))
    
    # Update layout for better visualization
    fig.update_layout(
        title = dict(
            text='<b>World Gold Deposits Map</b><br><sub>Major Gold Mining Districts and Deposit Types</sub>',
            x=0.5,
            xanchor='center'
        ),
        geo = dict(
            scope = 'world',
            showland = True,
            landcolor = 'lightgray',
            showocean = True,
            oceancolor = 'lightblue',
            showcountries = True,
            countrycolor = 'white',
            countrywidth = 0.5,
            projection_type = 'natural earth'
        ),
        height = 700,
        showlegend = True
    )
    
    fig.show()
    # Save as HTML
    fig.write_html("gold_deposits_map.html")
    print("Map saved as 'gold_deposits_map.html'")

# Run the function
create_gold_map_plotly()

#Method 2: Using Matplotlib with Cartopy (Professional Static Maps)

import matplotlib.pyplot as plt
import cartopy.crs as ccrs
import cartopy.feature as cfeature
import pandas as pd
import numpy as np

def create_gold_map_cartopy():
    # Create the figure and map
    fig = plt.figure(figsize=(16, 10))
    ax = plt.axes(projection=ccrs.Robinson())
    ax.set_global()
    
    # Add map features
    ax.add_feature(cfeature.LAND, color='lightgray')
    ax.add_feature(cfeature.OCEAN, color='lightblue')
    ax.add_feature(cfeature.COASTLINE, linewidth=0.5)
    ax.add_feature(cfeature.BORDERS, linewidth=0.3)
    
    # Gold deposits data
    deposits = [
        # Africa
        {'name': 'Witwatersrand', 'lat': -26.20, 'lon': 28.05, 'type': 'Basin'},
        {'name': 'Tarkwa', 'lat': 5.30, 'lon': -2.00, 'type': 'Orogenic'},
        {'name': 'Geita', 'lat': -2.87, 'lon': 32.23, 'type': 'Orogenic'},
        # North America
        {'name': 'Carlin', 'lat': 40.95, 'lon': -116.13, 'type': 'Carlin-type'},
        {'name': 'Homestake', 'lat': 44.35, 'lon': -103.75, 'type': 'Orogenic'},
        {'name': 'Red Lake', 'lat': 51.06, 'lon': -93.83, 'type': 'Orogenic'},
        # Australia
        {'name': 'Kalgoorlie', 'lat': -30.76, 'lon': 121.47, 'type': 'Orogenic'},
        {'name': 'Boddington', 'lat': -32.80, 'lon': 116.47, 'type': 'Orogenic'},
        # Asia
        {'name': 'Muruntau', 'lat': 41.55, 'lon': 64.61, 'type': 'Orogenic'},
        {'name': 'Grasberg', 'lat': -4.06, 'lon': 137.11, 'type': 'Porphyry'},
        # South America
        {'name': 'Yanacocha', 'lat': -6.48, 'lon': -78.35, 'type': 'Epithermal'},
    ]
    
    df = pd.DataFrame(deposits)
    
    # Convert coordinates to map projection
    transform = ccrs.PlateCarree()
    
    # Plot deposits with different markers and colors
    type_styles = {
        'Orogenic': {'color': 'red', 'marker': 'o', 'size': 60},
        'Carlin-type': {'color': 'blue', 'marker': 's', 'size': 60},
        'Porphyry': {'color': 'green', 'marker': '^', 'size': 60},
        'Epithermal': {'color': 'orange', 'marker': 'D', 'size': 60},
        'Basin': {'color': 'purple', 'marker': 'P', 'size': 80}
    }
    
    # Plot each deposit type
    for deposit_type, style in type_styles.items():
        subset = df[df['type'] == deposit_type]
        if len(subset) > 0:
            ax.scatter(subset['lon'], subset['lat'], 
                      c=style['color'], marker=style['marker'], s=style['size'],
                      transform=transform, label=deposit_type, 
                      edgecolors='black', linewidth=0.5, alpha=0.8)
    
    # Add tectonic context - Pacific Ring of Fire
    ring_lons = [-170, -150, -120, -75, -70, 140, 160, 180, -170]
    ring_lats = [60, 50, 30, -10, -30, -30, 50, 60, 60]
    ax.plot(ring_lons, ring_lats, color='red', linewidth=2, 
            transform=transform, label='Pacific Ring of Fire', alpha=0.7)
    
    # Add major craton annotations
    ax.text(-50, -25, 'Brazilian\nShield', transform=transform, 
            fontsize=8, ha='center', color='brown')
    ax.text(25, -10, 'African\nCraton', transform=transform, 
            fontsize=8, ha='center', color='brown')
    ax.text(120, -25, 'Yilgarn\nCraton', transform=transform, 
            fontsize=8, ha='center', color='brown')
    
    # Customize the map
    plt.title('World Gold Deposits and Geological Settings\nMajor Mining Districts and Tectonic Context', 
              fontsize=14, pad=20)
    plt.legend(loc='lower left', frameon=True, fancybox=True)
    
    # Add gridlines
    ax.gridlines(color='gray', alpha=0.3, draw_labels=True)
    
    plt.tight_layout()
    plt.savefig('gold_deposits_cartopy.png', dpi=300, bbox_inches='tight')
    plt.show()
    print("Map saved as 'gold_deposits_cartopy.png'")

create_gold_map_cartopy()

#Method 3: Simple Matplotlib with Basemap Alternative
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

def create_simple_gold_map():
    """Create a simple gold map using basic matplotlib"""
    
    # Create figure
    fig, ax = plt.subplots(figsize=(16, 8))
    
    # Simple world map background
    ax.set_facecolor('lightblue')
    ax.add_patch(plt.Rectangle((-180, -90), 360, 180, color='lightgray'))
    
    # Add continent outlines (simplified)
    continents = {
        'North America': [[-140, 70], [-60, 70], [-60, 10], [-140, 10]],
        'South America': [[-80, 10], [-35, 10], [-35, -55], [-80, -55]],
        'Africa': [[-20, 35], [50, 35], [50, -35], [-20, -35]],
        'Europe': [[-10, 70], [40, 70], [40, 35], [-10, 35]],
        'Asia': [[40, 70], [180, 70], [180, 10], [40, 10]],
        'Australia': [[110, -10], [155, -10], [155, -45], [110, -45]]
    }
    
    for continent, coords in continents.items():
        x = [coord[0] for coord in coords]
        y = [coord[1] for coord in coords]
        ax.fill(x, y, color='wheat', alpha=0.7, edgecolor='gray')
    
    # Gold deposits data
    deposits_data = {
        'name': ['Witwatersrand', 'Carlin', 'Kalgoorlie', 'Grasberg', 'Muruntau', 
                'Yanacocha', 'Homestake', 'Red Lake', 'Tarkwa'],
        'lon': [28.05, -116.13, 121.47, 137.11, 64.61, -78.35, -103.75, -93.83, -2.00],
        'lat': [-26.20, 40.95, -30.76, -4.06, 41.55, -6.48, 44.35, 51.06, 5.30],
        'type': ['Basin', 'Carlin-type', 'Orogenic', 'Porphyry', 'Orogenic', 
                'Epithermal', 'Orogenic', 'Orogenic', 'Orogenic']
    }
    
    df = pd.DataFrame(deposits_data)
    
    # Plot deposits
    type_colors = {'Orogenic': 'red', 'Carlin-type': 'blue', 'Porphyry': 'green', 
                  'Epithermal': 'orange', 'Basin': 'purple'}
    
    for deposit_type, color in type_colors.items():
        subset = df[df['type'] == deposit_type]
        ax.scatter(subset['lon'], subset['lat'], c=color, s=100, 
                  label=deposit_type, edgecolors='black', alpha=0.8)
        
        # Add labels for major deposits
        for _, row in subset.iterrows():
            ax.annotate(row['name'], (row['lon'], row['lat']), 
                       xytext=(5, 5), textcoords='offset points', fontsize=8)
    
    # Customize the map
    ax.set_xlim(-180, 180)
    ax.set_ylim(-60, 80)
    ax.set_xlabel('Longitude')
    ax.set_ylabel('Latitude')
    ax.set_title('World Gold Deposits Distribution\nMajor Mining Districts by Deposit Type', fontsize=14)
    ax.legend(loc='upper right')
    ax.grid(True, alpha=0.3)
    
    plt.tight_layout()
    plt.savefig('simple_gold_map.png', dpi=300, bbox_inches='tight')
    plt.show()
    print("Map saved as 'simple_gold_map.png'")

create_simple_gold_map()

#Installation Commands (Choose one):
# For Plotly method (easiest and most reliable):
pip install plotly pandas

# For Cartopy method (professional maps):
pip install cartopy matplotlib pandas

# For simple method (minimum dependencies):
pip install matplotlib pandas numpy
