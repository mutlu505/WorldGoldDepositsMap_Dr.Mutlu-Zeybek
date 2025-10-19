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

#Method 1: Plotly with Gold Amounts
import plotly.graph_objects as go
import pandas as pd

def create_gold_deposits_with_amounts():
    deposits = [
        # Africa
        {'name': 'Witwatersrand Basin', 'lat': -26.2044, 'lon': 28.0456, 'type': 'Basin', 
         'production': 2.0, 'reserves': 30.0, 'units': 'Boz', 'country': 'South Africa'},
        
        {'name': 'Tarkwa', 'lat': 5.30, 'lon': -2.00, 'type': 'Orogenic', 
         'production': 0.6, 'reserves': 15.2, 'units': 'Moz', 'country': 'Ghana'},
        
        {'name': 'Geita', 'lat': -2.87, 'lon': 32.23, 'type': 'Orogenic', 
         'production': 0.5, 'reserves': 8.5, 'units': 'Moz', 'country': 'Tanzania'},
        
        {'name': 'Loulo-Gounkoto', 'lat': 13.50, 'lon': -7.50, 'type': 'Orogenic', 
         'production': 0.6, 'reserves': 10.1, 'units': 'Moz', 'country': 'Mali'},

        # North America
        {'name': 'Carlin Trend', 'lat': 40.95, 'lon': -116.13, 'type': 'Carlin-type', 
         'production': 1.8, 'reserves': 25.0, 'units': 'Moz', 'country': 'USA'},
        
        {'name': 'Goldstrike', 'lat': 40.85, 'lon': -116.70, 'type': 'Carlin-type', 
         'production': 1.1, 'reserves': 12.3, 'units': 'Moz', 'country': 'USA'},
        
        {'name': 'Cortez', 'lat': 39.74, 'lon': -116.87, 'type': 'Carlin-type', 
         'production': 0.8, 'reserves': 9.5, 'units': 'Moz', 'country': 'USA'},
        
        {'name': 'Porcupine', 'lat': 48.47, 'lon': -81.20, 'type': 'Orogenic', 
         'production': 0.3, 'reserves': 7.2, 'units': 'Moz', 'country': 'Canada'},

        # Australia
        {'name': 'Super Pit Kalgoorlie', 'lat': -30.76, 'lon': 121.47, 'type': 'Orogenic', 
         'production': 0.6, 'reserves': 8.5, 'units': 'Moz', 'country': 'Australia'},
        
        {'name': 'Boddington', 'lat': -32.80, 'lon': 116.47, 'type': 'Orogenic', 
         'production': 0.7, 'reserves': 15.8, 'units': 'Moz', 'country': 'Australia'},
        
        {'name': 'Olympic Dam', 'lat': -30.43, 'lon': 136.88, 'type': 'IOCG', 
         'production': 0.2, 'reserves': 86.0, 'units': 'Moz', 'country': 'Australia'},

        # Asia
        {'name': 'Muruntau', 'lat': 41.55, 'lon': 64.61, 'type': 'Orogenic', 
         'production': 2.4, 'reserves': 150.0, 'units': 'Moz', 'country': 'Uzbekistan'},
        
        {'name': 'Grasberg', 'lat': -4.06, 'lon': 137.11, 'type': 'Porphyry', 
         'production': 1.5, 'reserves': 25.8, 'units': 'Moz', 'country': 'Indonesia'},
        
        {'name': 'Olimpiada', 'lat': 58.40, 'lon': 95.30, 'type': 'Orogenic', 
         'production': 1.3, 'reserves': 42.5, 'units': 'Moz', 'country': 'Russia'},
        
        {'name': 'Pueblo Viejo', 'lat': 18.98, 'lon': -70.15, 'type': 'Epithermal', 
         'production': 0.8, 'reserves': 16.5, 'units': 'Moz', 'country': 'Dominican Republic'},

        # South America
        {'name': 'Yanacocha', 'lat': -6.48, 'lon': -78.35, 'type': 'Epithermal', 
         'production': 0.5, 'reserves': 7.3, 'units': 'Moz', 'country': 'Peru'},
        
        {'name': 'Veladero', 'lat': -29.33, 'lon': -69.83, 'type': 'Epithermal', 
         'production': 0.4, 'reserves': 6.8, 'units': 'Moz', 'country': 'Argentina'},
        
        {'name': 'Paracatu', 'lat': -17.22, 'lon': -46.87, 'type': 'Orogenic', 
         'production': 0.5, 'reserves': 12.4, 'units': 'Moz', 'country': 'Brazil'},

        # Papua New Guinea
        {'name': 'Lihir', 'lat': -3.12, 'lon': 152.64, 'type': 'Epithermal', 
         'production': 0.8, 'reserves': 35.0, 'units': 'Moz', 'country': 'PNG'},
    ]
    
    return pd.DataFrame(deposits)

def create_gold_map_with_amounts():
    df = create_gold_deposits_with_amounts()
    
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
    
    # Create hover text with detailed information
    df['hover_text'] = df.apply(lambda x: 
        f"<b>{x['name']}</b><br>"
        f"Country: {x['country']}<br>"
        f"Type: {x['type']}<br>"
        f"Annual Production: {x['production']} {x['units']}<br>"
        f"Reserves: {x['reserves']} {x['units']}", axis=1)
    
    # Create the map
    fig = go.Figure()
    
    # Add deposits by type
    for deposit_type in df['type'].unique():
        df_subset = df[df['type'] == deposit_type]
        
        # Size based on reserves
        sizes = df_subset['reserves'].apply(lambda x: min(50, max(10, x/3)))
        
        fig.add_trace(go.Scattergeo(
            lon = df_subset['lon'],
            lat = df_subset['lat'],
            text = df_subset['hover_text'],
            name = deposit_type,
            marker = dict(
                size = sizes,
                color = color_map[deposit_type],
                line = dict(width=1, color='black'),
                sizemode = 'area'
            ),
            hovertemplate = '%{text}<extra></extra>'
        ))
    
    # Update layout
    fig.update_layout(
        title = dict(
            text='<b>World Gold Deposits with Production & Reserve Data</b><br><sub>Circle size indicates reserve size | Hover for details</sub>',
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
    fig.write_html("gold_deposits_with_amounts.html")
    print("Interactive map saved as 'gold_deposits_with_amounts.html'")
    
    # Also create a summary table
    print("\n" + "="*80)
    print("MAJOR GOLD DEPOSITS SUMMARY (Top 10 by Reserves)")
    print("="*80)
    
    summary_df = df.nlargest(10, 'reserves')[['name', 'country', 'type', 'production', 'reserves', 'units']]
    for _, row in summary_df.iterrows():
        print(f"{row['name']:30} | {row['country']:15} | {row['type']:12} | "
              f"Prod: {row['production']:4.1f} {row['units']} | Reserves: {row['reserves']:6.1f} {row['units']}")

create_gold_map_with_amounts()

#Method 2: Detailed Summary Table with All Data
import pandas as pd

def create_detailed_gold_table():
    """Create a comprehensive table of gold deposits with detailed amounts"""
    
    deposits_detailed = [
        {
            'Deposit': 'Witwatersrand Basin',
            'Country': 'South Africa',
            'Type': 'Basin',
            'Annual Production (Moz)': 2.0,
            'Total Reserves (Moz)': 30000,  # 30 billion ounces historical production
            'Cumulative Production (Moz)': 50000,
            'Grade (g/t)': 8-10,
            'Status': 'Producing'
        },
        {
            'Deposit': 'Muruntau',
            'Country': 'Uzbekistan',
            'Type': 'Orogenic',
            'Annual Production (Moz)': 2.4,
            'Total Reserves (Moz)': 150.0,
            'Cumulative Production (Moz)': 120.0,
            'Grade (g/t)': 2.5,
            'Status': 'Producing'
        },
        {
            'Deposit': 'Grasberg',
            'Country': 'Indonesia',
            'Type': 'Porphyry',
            'Annual Production (Moz)': 1.5,
            'Total Reserves (Moz)': 25.8,
            'Cumulative Production (Moz)': 55.0,
            'Grade (g/t)': 0.8,
            'Status': 'Producing'
        },
        {
            'Deposit': 'Carlin Trend',
            'Country': 'USA',
            'Type': 'Carlin-type',
            'Annual Production (Moz)': 1.8,
            'Total Reserves (Moz)': 25.0,
            'Cumulative Production (Moz)': 95.0,
            'Grade (g/t)': 1.5,
            'Status': 'Producing'
        },
        {
            'Deposit': 'Olympic Dam',
            'Country': 'Australia',
            'Type': 'IOCG',
            'Annual Production (Moz)': 0.2,
            'Total Reserves (Moz)': 86.0,
            'Cumulative Production (Moz)': 25.0,
            'Grade (g/t)': 0.5,
            'Status': 'Producing'
        },
        {
            'Deposit': 'Lihir',
            'Country': 'Papua New Guinea',
            'Type': 'Epithermal',
            'Annual Production (Moz)': 0.8,
            'Total Reserves (Moz)': 35.0,
            'Cumulative Production (Moz)': 20.0,
            'Grade (g/t)': 2.4,
            'Status': 'Producing'
        },
        {
            'Deposit': 'Olimpiada',
            'Country': 'Russia',
            'Type': 'Orogenic',
            'Annual Production (Moz)': 1.3,
            'Total Reserves (Moz)': 42.5,
            'Cumulative Production (Moz)': 35.0,
            'Grade (g/t)': 3.8,
            'Status': 'Producing'
        },
        {
            'Deposit': 'Boddington',
            'Country': 'Australia',
            'Type': 'Orogenic',
            'Annual Production (Moz)': 0.7,
            'Total Reserves (Moz)': 15.8,
            'Cumulative Production (Moz)': 12.0,
            'Grade (g/t)': 0.8,
            'Status': 'Producing'
        },
        {
            'Deposit': 'Pueblo Viejo',
            'Country': 'Dominican Republic',
            'Type': 'Epithermal',
            'Annual Production (Moz)': 0.8,
            'Total Reserves (Moz)': 16.5,
            'Cumulative Production (Moz)': 8.0,
            'Grade (g/t)': 2.5,
            'Status': 'Producing'
        },
        {
            'Deposit': 'Kibali',
            'Country': 'DRC',
            'Type': 'Orogenic',
            'Annual Production (Moz)': 0.8,
            'Total Reserves (Moz)': 11.5,
            'Cumulative Production (Moz)': 6.5,
            'Grade (g/t)': 3.5,
            'Status': 'Producing'
        }
    ]
    
    df = pd.DataFrame(deposits_detailed)
    
    # Display the table
    print("COMPREHENSIVE GOLD DEPOSITS DATABASE")
    print("="*100)
    print(f"{'Deposit':<25} {'Country':<15} {'Type':<12} {'Annual Prod (Moz)':<18} {'Reserves (Moz)':<15} {'Grade (g/t)':<12} {'Status':<10}")
    print("-"*100)
    
    for _, row in df.iterrows():
        print(f"{row['Deposit']:<25} {row['Country']:<15} {row['Type']:<12} "
              f"{row['Annual Production (Moz)']:<18.1f} {row['Total Reserves (Moz)']:<15.1f} "
              f"{row['Grade (g/t)']:<12} {row['Status']:<10}")
    
    # Summary statistics
    print("\n" + "="*100)
    print("SUMMARY STATISTICS")
    print("="*100)
    print(f"Total Annual Production from Major Deposits: {df['Annual Production (Moz)'].sum():.1f} Moz")
    print(f"Total Reserves in Major Deposits: {df['Total Reserves (Moz)'].sum():.1f} Moz")
    print(f"Average Grade: {df['Grade (g/t)'].mean():.1f} g/t")
    print(f"Number of Major Deposits: {len(df)}")
    
    # By country summary
    country_summary = df.groupby('Country').agg({
        'Annual Production (Moz)': 'sum',
        'Total Reserves (Moz)': 'sum',
        'Deposit': 'count'
    }).round(1)
    
    print("\nBY COUNTRY SUMMARY:")
    print(country_summary.sort_values('Total Reserves (Moz)', ascending=False))
    
    return df

# Create the detailed table
detailed_df = create_detailed_gold_table()

#Method 3: Regional Breakdown with Gold Amounts
import matplotlib.pyplot as plt
import numpy as np

def create_regional_gold_analysis():
    """Create regional analysis of gold deposits and amounts"""
    
    regional_data = {
        'Region': ['Africa', 'North America', 'Asia', 'Australia', 'South America', 'Papua New Guinea'],
        'Annual Production (Moz)': [12.5, 8.2, 15.3, 5.8, 4.2, 1.8],
        'Total Reserves (Moz)': [450.0, 280.0, 620.0, 180.0, 120.0, 85.0],
        'Major Deposits Count': [25, 18, 22, 12, 15, 8]
    }
    
    # Create visualization
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))
    
    # Plot 1: Annual Production by Region
    regions = regional_data['Region']
    production = regional_data['Annual Production (Moz)']
    
    bars1 = ax1.bar(regions, production, color=['gold', 'orange', 'red', 'yellow', 'green', 'brown'])
    ax1.set_title('Annual Gold Production by Region\n(Million Ounces)', fontsize=14, fontweight='bold')
    ax1.set_ylabel('Million Ounces (Moz)')
    ax1.tick_params(axis='x', rotation=45)
    
    # Add value labels on bars
    for bar in bars1:
        height = bar.get_height()
        ax1.text(bar.get_x() + bar.get_width()/2., height,
                f'{height:.1f} Moz',
                ha='center', va='bottom')
    
    # Plot 2: Total Reserves by Region
    reserves = regional_data['Total Reserves (Moz)']
    
    bars2 = ax2.bar(regions, reserves, color=['gold', 'orange', 'red', 'yellow', 'green', 'brown'])
    ax2.set_title('Total Gold Reserves by Region\n(Million Ounces)', fontsize=14, fontweight='bold')
    ax2.set_ylabel('Million Ounces (Moz)')
    ax2.tick_params(axis='x', rotation=45)
    
    # Add value labels on bars
    for bar in bars2:
        height = bar.get_height()
        ax2.text(bar.get_x() + bar.get_width()/2., height,
                f'{height:.0f} Moz',
                ha='center', va='bottom')
    
    plt.tight_layout()
    plt.savefig('regional_gold_analysis.png', dpi=300, bbox_inches='tight')
    plt.show()
    
    # Print regional summary
    print("REGIONAL GOLD PRODUCTION AND RESERVES")
    print("="*70)
    print(f"{'Region':<20} {'Annual Production (Moz)':<25} {'Total Reserves (Moz)':<20} {'Major Deposits':<15}")
    print("-"*70)
    
    for i in range(len(regional_data['Region'])):
        print(f"{regional_data['Region'][i]:<20} {regional_data['Annual Production (Moz)'][i]:<25.1f} "
              f"{regional_data['Total Reserves (Moz)'][i]:<20.0f} {regional_data['Major Deposits Count'][i]:<15}")

# Run regional analysis
create_regional_gold_analysis()

#Method 1: Using Plotly for Interactive Visualization
import plotly.graph_objects as go
import plotly.express as px
import pandas as pd
import numpy as np

def create_gold_deposits_map():
    """
    Create an interactive world map of major gold deposits using USGS data
    """
    
    # Major gold deposits data from USGS and company reports
    gold_data = [
        # North America
        {'name': 'Carlin Trend', 'lat': 40.95, 'lon': -116.13, 'type': 'Carlin-type', 
         'reserves': 25.0, 'production': 1.8, 'country': 'USA', 'geology': 'Paleozoic carbonates',
         'tectonic_setting': 'Intracontinental rift', 'age': 'Cenozoic'},
        
        {'name': 'Homestake', 'lat': 44.35, 'lon': -103.75, 'type': 'Orogenic', 
         'reserves': 7.5, 'production': 0.4, 'country': 'USA', 'geology': 'Precambrian metamorphic',
         'tectonic_setting': 'Cratonic margin', 'age': 'Precambrian'},
        
        {'name': 'Red Lake', 'lat': 51.06, 'lon': -93.83, 'type': 'Orogenic', 
         'reserves': 8.2, 'production': 0.5, 'country': 'Canada', 'geology': 'Archean greenstone',
         'tectonic_setting': 'Superior Craton', 'age': 'Archean'},
        
        # South America
        {'name': 'Yanacocha', 'lat': -6.48, 'lon': -78.35, 'type': 'Epithermal', 
         'reserves': 7.3, 'production': 0.5, 'country': 'Peru', 'geology': 'Andean volcanic belt',
         'tectonic_setting': 'Convergent margin', 'age': 'Cenozoic'},
        
        {'name': 'Paracatu', 'lat': -17.22, 'lon': -46.87, 'type': 'Orogenic', 
         'reserves': 12.4, 'production': 0.5, 'country': 'Brazil', 'geology': 'Brazilian Shield',
         'tectonic_setting': 'Cratonic', 'age': 'Proterozoic'},
        
        # Africa
        {'name': 'Witwatersrand Basin', 'lat': -26.20, 'lon': 28.05, 'type': 'Paleoplacer', 
         'reserves': 30000.0, 'production': 2.0, 'country': 'South Africa', 'geology': 'Archean basin',
         'tectonic_setting': 'Cratonic basin', 'age': 'Archean'},
        
        {'name': 'Tarkwa', 'lat': 5.30, 'lon': -2.00, 'type': 'Orogenic', 
         'reserves': 15.2, 'production': 0.6, 'country': 'Ghana', 'geology': 'Birimian greenstone',
         'tectonic_setting': 'West African Craton', 'age': 'Proterozoic'},
        
        {'name': 'Geita', 'lat': -2.87, 'lon': 32.23, 'type': 'Orogenic', 
         'reserves': 8.5, 'production': 0.5, 'country': 'Tanzania', 'geology': 'Lake Victoria greenstone',
         'tectonic_setting': 'Tanzania Craton', 'age': 'Archean'},
        
        # Asia
        {'name': 'Muruntau', 'lat': 41.55, 'lon': 64.61, 'type': 'Orogenic', 
         'reserves': 150.0, 'production': 2.4, 'country': 'Uzbekistan', 'geology': 'Paleozoic metamorphic',
         'tectonic_setting': 'Tien Shan orogen', 'age': 'Paleozoic'},
        
        {'name': 'Grasberg', 'lat': -4.06, 'lon': 137.11, 'type': 'Porphyry', 
         'reserves': 25.8, 'production': 1.5, 'country': 'Indonesia', 'geology': 'Porphyry copper-gold',
         'tectonic_setting': 'Convergent margin', 'age': 'Cenozoic'},
        
        {'name': 'Olimpiada', 'lat': 58.40, 'lon': 95.30, 'type': 'Orogenic', 
         'reserves': 42.5, 'production': 1.3, 'country': 'Russia', 'geology': 'Siberian craton',
         'tectonic_setting': 'Cratonic margin', 'age': 'Proterozoic'},
        
        # Australia
        {'name': 'Super Pit Kalgoorlie', 'lat': -30.76, 'lon': 121.47, 'type': 'Orogenic', 
         'reserves': 8.5, 'production': 0.6, 'country': 'Australia', 'geology': 'Yilgarn greenstone',
         'tectonic_setting': 'Yilgarn Craton', 'age': 'Archean'},
        
        {'name': 'Boddington', 'lat': -32.80, 'lon': 116.47, 'type': 'Orogenic', 
         'reserves': 15.8, 'production': 0.7, 'country': 'Australia', 'geology': 'Archean greenstone',
         'tectonic_setting': 'Yilgarn Craton', 'age': 'Archean'},
        
        {'name': 'Olympic Dam', 'lat': -30.43, 'lon': 136.88, 'type': 'IOCG', 
         'reserves': 86.0, 'production': 0.2, 'country': 'Australia', 'geology': 'Proterozoic breccia',
         'tectonic_setting': 'Gawler Craton', 'age': 'Proterozoic'},
        
        # Papua New Guinea
        {'name': 'Lihir', 'lat': -3.12, 'lon': 152.64, 'type': 'Epithermal', 
         'reserves': 35.0, 'production': 0.8, 'country': 'Papua New Guinea', 'geology': 'Volcanic caldera',
         'tectonic_setting': 'Island arc', 'age': 'Cenozoic'},
    ]
    
    df = pd.DataFrame(gold_data)
    
    # Color mapping by deposit type
    color_map = {
        'Orogenic': 'red',
        'Carlin-type': 'blue',
        'Porphyry': 'green',
        'Epithermal': 'orange',
        'Paleoplacer': 'purple',
        'IOCG': 'brown'
    }
    
    # Create hover text
    df['hover_text'] = df.apply(lambda x: 
        f"<b>{x['name']}</b><br>"
        f"Country: {x['country']}<br>"
        f"Type: {x['type']}<br>"
        f"Reserves: {x['reserves']} Moz<br>"
        f"Annual Production: {x['production']} Moz<br>"
        f"Geology: {x['geology']}<br>"
        f"Tectonic Setting: {x['tectonic_setting']}<br>"
        f"Age: {x['age']}", axis=1)
    
    # Create the map
    fig = go.Figure()
    
    # Add gold deposits by type
    for deposit_type in df['type'].unique():
        df_subset = df[df['type'] == deposit_type]
        
        # Adjust size for Witwatersrand (too large for visualization)
        sizes = df_subset['reserves'].apply(lambda x: min(50, max(10, np.log(x + 1) * 8)))
        
        fig.add_trace(go.Scattergeo(
            lon = df_subset['lon'],
            lat = df_subset['lat'],
            text = df_subset['hover_text'],
            name = f'{deposit_type} Deposits',
            marker = dict(
                size = sizes,
                color = color_map[deposit_type],
                line = dict(width=1, color='black'),
                sizemode = 'diameter',
                opacity = 0.8
            ),
            hovertemplate = '%{text}<extra></extra>'
        ))
    
    # Add tectonic boundaries (simplified)
    tectonic_features = [
        # Pacific Ring of Fire
        {'name': 'Pacific Ring of Fire', 'lons': [-170, -150, -120, -75, -70, 140, 160, 180, -170], 
         'lats': [60, 50, 30, -10, -30, -30, 50, 60, 60], 'color': 'red'},
        
        # Alpine-Himalayan Belt
        {'name': 'Alpine-Himalayan Belt', 'lons': [-5, 10, 25, 40, 60, 80, 100, 120], 
         'lats': [45, 45, 40, 35, 30, 30, 25, 20], 'color': 'orange'},
    ]
    
    for feature in tectonic_features:
        fig.add_trace(go.Scattergeo(
            lon = feature['lons'],
            lat = feature['lats'],
            mode = 'lines',
            line = dict(width=2, color=feature['color']),
            name = feature['name'],
            opacity = 0.6
        ))
    
    # Update layout
    fig.update_layout(
        title = dict(
            text='<b>Global Distribution of Major Gold Deposits</b><br>'
                 '<sub>Based on USGS Data and Geological Framework</sub>',
            x=0.5,
            xanchor='center',
            font=dict(size=16)
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
            showframe = False,
            projection_type = 'natural earth'
        ),
        height = 700,
        showlegend = True,
        legend = dict(
            yanchor="top",
            y=0.99,
            xanchor="left",
            x=0.01
        )
    )
    
    fig.show()
    fig.write_html("global_gold_deposits_map.html")
    print("Interactive map saved as 'global_gold_deposits_map.html'")
    
    return df

# Create the map
gold_df = create_gold_deposits_map()

#Method 2: Using Matplotlib with Cartopy for Publication-Quality Map
import matplotlib.pyplot as plt
import cartopy.crs as ccrs
import cartopy.feature as cfeature
import pandas as pd
import numpy as np
from matplotlib.patches import Patch
import matplotlib.colors as mcolors

def create_geological_gold_map():
    """
    Create a geological map of gold deposits with tectonic context
    """
    
    # Create figure with multiple subplots
    fig = plt.figure(figsize=(20, 12))
    
    # Main map
    ax = plt.axes(projection=ccrs.Robinson())
    ax.set_global()
    
    # Add base features
    ax.add_feature(cfeature.LAND, color='lightgray', alpha=0.8)
    ax.add_feature(cfeature.OCEAN, color='lightblue', alpha=0.5)
    ax.add_feature(cfeature.COASTLINE, linewidth=0.5)
    ax.add_feature(cfeature.BORDERS, linewidth=0.3, alpha=0.5)
    
    # Gold deposits data (using same data as above)
    deposits = [
        # North America
        {'name': 'Carlin Trend', 'lat': 40.95, 'lon': -116.13, 'type': 'Carlin-type', 'reserves': 25.0},
        {'name': 'Homestake', 'lat': 44.35, 'lon': -103.75, 'type': 'Orogenic', 'reserves': 7.5},
        # South America
        {'name': 'Yanacocha', 'lat': -6.48, 'lon': -78.35, 'type': 'Epithermal', 'reserves': 7.3},
        {'name': 'Paracatu', 'lat': -17.22, 'lon': -46.87, 'type': 'Orogenic', 'reserves': 12.4},
        # Africa
        {'name': 'Witwatersrand', 'lat': -26.20, 'lon': 28.05, 'type': 'Paleoplacer', 'reserves': 30000.0},
        {'name': 'Tarkwa', 'lat': 5.30, 'lon': -2.00, 'type': 'Orogenic', 'reserves': 15.2},
        # Asia
        {'name': 'Muruntau', 'lat': 41.55, 'lon': 64.61, 'type': 'Orogenic', 'reserves': 150.0},
        {'name': 'Grasberg', 'lat': -4.06, 'lon': 137.11, 'type': 'Porphyry', 'reserves': 25.8},
        # Australia
        {'name': 'Kalgoorlie', 'lat': -30.76, 'lon': 121.47, 'type': 'Orogenic', 'reserves': 8.5},
        {'name': 'Boddington', 'lat': -32.80, 'lon': 116.47, 'type': 'Orogenic', 'reserves': 15.8},
    ]
    
    df = pd.DataFrame(deposits)
    
    # Plot deposits
    type_markers = {
        'Orogenic': 'o',
        'Carlin-type': 's',
        'Porphyry': '^',
        'Epithermal': 'D',
        'Paleoplacer': 'P'
    }
    
    type_colors = {
        'Orogenic': 'red',
        'Carlin-type': 'blue',
        'Porphyry': 'green',
        'Epithermal': 'orange',
        'Paleoplacer': 'purple'
    }
    
    transform = ccrs.PlateCarree()
    
    for _, deposit in df.iterrows():
        # Adjust size for visualization (log scale for Witwatersrand)
        size = min(200, max(30, np.log(deposit['reserves'] + 1) * 20))
        
        ax.scatter(deposit['lon'], deposit['lat'],
                  marker=type_markers[deposit['type']],
                  s=size,
                  c=type_colors[deposit['type']],
                  edgecolors='black',
                  linewidth=1,
                  transform=transform,
                  alpha=0.8,
                  label=deposit['type'] if deposit['name'] == 'Carlin Trend' else "")
        
        # Add labels for major deposits
        if deposit['reserves'] > 20:
            ax.text(deposit['lon'] + 2, deposit['lat'] + 1, deposit['name'],
                   transform=transform, fontsize=8, ha='left')
    
    # Add tectonic context
    # Pacific Ring of Fire
    ring_lons = [-170, -150, -120, -75, -70, 140, 160, 180, -170]
    ring_lats = [60, 50, 30, -10, -30, -30, 50, 60, 60]
    ax.plot(ring_lons, ring_lats, color='red', linewidth=2,
            transform=transform, label='Pacific Ring of Fire', alpha=0.7)
    
    # Major cratons annotation
    cratons = [
        ('African Craton', 20, -5),
        ('Brazilian Shield', -45, -15),
        ('Canadian Shield', -85, 55),
        ('Yilgarn Craton', 120, -30),
        ('Siberian Craton', 100, 65)
    ]
    
    for craton, lon, lat in cratons:
        ax.text(lon, lat, craton, transform=transform,
               fontsize=8, ha='center', style='italic', alpha=0.7)
    
    # Create custom legend
    legend_elements = [
        Patch(facecolor='red', edgecolor='black', label='Orogenic'),
        Patch(facecolor='blue', edgecolor='black', label='Carlin-type'),
        Patch(facecolor='green', edgecolor='black', label='Porphyry'),
        Patch(facecolor='orange', edgecolor='black', label='Epithermal'),
        Patch(facecolor='purple', edgecolor='black', label='Paleoplacer'),
    ]
    
    ax.legend(handles=legend_elements, loc='lower left', frameon=True)
    
    plt.title('Global Gold Deposits and Tectonic Framework\n'
             'Data Source: USGS Mineral Resources Program', 
             fontsize=16, pad=20)
    
    plt.tight_layout()
    plt.savefig('geological_gold_map.png', dpi=300, bbox_inches='tight')
    plt.show()

create_geological_gold_map()

#Method 2: Using Matplotlib with Cartopy for Publication-Quality Map
import matplotlib.pyplot as plt
import cartopy.crs as ccrs
import cartopy.feature as cfeature
import pandas as pd
import numpy as np
from matplotlib.patches import Patch
import matplotlib.colors as mcolors

def create_geological_gold_map():
    """
    Create a geological map of gold deposits with tectonic context
    """
    
    # Create figure with multiple subplots
    fig = plt.figure(figsize=(20, 12))
    
    # Main map
    ax = plt.axes(projection=ccrs.Robinson())
    ax.set_global()
    
    # Add base features
    ax.add_feature(cfeature.LAND, color='lightgray', alpha=0.8)
    ax.add_feature(cfeature.OCEAN, color='lightblue', alpha=0.5)
    ax.add_feature(cfeature.COASTLINE, linewidth=0.5)
    ax.add_feature(cfeature.BORDERS, linewidth=0.3, alpha=0.5)
    
    # Gold deposits data (using same data as above)
    deposits = [
        # North America
        {'name': 'Carlin Trend', 'lat': 40.95, 'lon': -116.13, 'type': 'Carlin-type', 'reserves': 25.0},
        {'name': 'Homestake', 'lat': 44.35, 'lon': -103.75, 'type': 'Orogenic', 'reserves': 7.5},
        # South America
        {'name': 'Yanacocha', 'lat': -6.48, 'lon': -78.35, 'type': 'Epithermal', 'reserves': 7.3},
        {'name': 'Paracatu', 'lat': -17.22, 'lon': -46.87, 'type': 'Orogenic', 'reserves': 12.4},
        # Africa
        {'name': 'Witwatersrand', 'lat': -26.20, 'lon': 28.05, 'type': 'Paleoplacer', 'reserves': 30000.0},
        {'name': 'Tarkwa', 'lat': 5.30, 'lon': -2.00, 'type': 'Orogenic', 'reserves': 15.2},
        # Asia
        {'name': 'Muruntau', 'lat': 41.55, 'lon': 64.61, 'type': 'Orogenic', 'reserves': 150.0},
        {'name': 'Grasberg', 'lat': -4.06, 'lon': 137.11, 'type': 'Porphyry', 'reserves': 25.8},
        # Australia
        {'name': 'Kalgoorlie', 'lat': -30.76, 'lon': 121.47, 'type': 'Orogenic', 'reserves': 8.5},
        {'name': 'Boddington', 'lat': -32.80, 'lon': 116.47, 'type': 'Orogenic', 'reserves': 15.8},
    ]
    
    df = pd.DataFrame(deposits)
    
    # Plot deposits
    type_markers = {
        'Orogenic': 'o',
        'Carlin-type': 's',
        'Porphyry': '^',
        'Epithermal': 'D',
        'Paleoplacer': 'P'
    }
    
    type_colors = {
        'Orogenic': 'red',
        'Carlin-type': 'blue',
        'Porphyry': 'green',
        'Epithermal': 'orange',
        'Paleoplacer': 'purple'
    }
    
    transform = ccrs.PlateCarree()
    
    for _, deposit in df.iterrows():
        # Adjust size for visualization (log scale for Witwatersrand)
        size = min(200, max(30, np.log(deposit['reserves'] + 1) * 20))
        
        ax.scatter(deposit['lon'], deposit['lat'],
                  marker=type_markers[deposit['type']],
                  s=size,
                  c=type_colors[deposit['type']],
                  edgecolors='black',
                  linewidth=1,
                  transform=transform,
                  alpha=0.8,
                  label=deposit['type'] if deposit['name'] == 'Carlin Trend' else "")
        
        # Add labels for major deposits
        if deposit['reserves'] > 20:
            ax.text(deposit['lon'] + 2, deposit['lat'] + 1, deposit['name'],
                   transform=transform, fontsize=8, ha='left')
    
    # Add tectonic context
    # Pacific Ring of Fire
    ring_lons = [-170, -150, -120, -75, -70, 140, 160, 180, -170]
    ring_lats = [60, 50, 30, -10, -30, -30, 50, 60, 60]
    ax.plot(ring_lons, ring_lats, color='red', linewidth=2,
            transform=transform, label='Pacific Ring of Fire', alpha=0.7)
    
    # Major cratons annotation
    cratons = [
        ('African Craton', 20, -5),
        ('Brazilian Shield', -45, -15),
        ('Canadian Shield', -85, 55),
        ('Yilgarn Craton', 120, -30),
        ('Siberian Craton', 100, 65)
    ]
    
    for craton, lon, lat in cratons:
        ax.text(lon, lat, craton, transform=transform,
               fontsize=8, ha='center', style='italic', alpha=0.7)
    
    # Create custom legend
    legend_elements = [
        Patch(facecolor='red', edgecolor='black', label='Orogenic'),
        Patch(facecolor='blue', edgecolor='black', label='Carlin-type'),
        Patch(facecolor='green', edgecolor='black', label='Porphyry'),
        Patch(facecolor='orange', edgecolor='black', label='Epithermal'),
        Patch(facecolor='purple', edgecolor='black', label='Paleoplacer'),
    ]
    
    ax.legend(handles=legend_elements, loc='lower left', frameon=True)
    
    plt.title('Global Gold Deposits and Tectonic Framework\n'
             'Data Source: USGS Mineral Resources Program', 
             fontsize=16, pad=20)
    
    plt.tight_layout()
    plt.savefig('geological_gold_map.png', dpi=300, bbox_inches='tight')
    plt.show()

create_geological_gold_map()

#Method 3: Data Analysis and Statistics
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

def analyze_gold_data():
    """
    Analyze gold deposit data and create statistical visualizations
    """
    
    # Create comprehensive dataset
    gold_analysis_data = [
        {'region': 'Africa', 'deposit_count': 25, 'total_reserves': 450, 'dominant_type': 'Orogenic'},
        {'region': 'Asia', 'deposit_count': 22, 'total_reserves': 620, 'dominant_type': 'Orogenic'},
        {'region': 'North America', 'deposit_count': 18, 'total_reserves': 280, 'dominant_type': 'Carlin-type'},
        {'region': 'Australia', 'deposit_count': 12, 'total_reserves': 180, 'dominant_type': 'Orogenic'},
        {'region': 'South America', 'deposit_count': 15, 'total_reserves': 120, 'dominant_type': 'Epithermal'},
    ]
    
    analysis_df = pd.DataFrame(gold_analysis_data)
    
    # Create subplots
    fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(15, 12))
    
    # Plot 1: Reserves by region
    ax1.bar(analysis_df['region'], analysis_df['total_reserves'], 
            color=['gold', 'orange', 'red', 'yellow', 'green'])
    ax1.set_title('Gold Reserves by Region (Moz)')
    ax1.set_ylabel('Million Ounces')
    ax1.tick_params(axis='x', rotation=45)
    
    # Plot 2: Deposit count by region
    ax2.pie(analysis_df['deposit_count'], labels=analysis_df['region'], autopct='%1.1f%%')
    ax2.set_title('Distribution of Major Gold Deposits')
    
    # Plot 3: Reserve distribution by type
    type_data = {'Orogenic': 65, 'Carlin-type': 15, 'Porphyry': 10, 'Epithermal': 8, 'Paleoplacer': 2}
    ax3.bar(type_data.keys(), type_data.values(), color=['red', 'blue', 'green', 'orange', 'purple'])
    ax3.set_title('Gold Reserves by Deposit Type (%)')
    ax3.set_ylabel('Percentage')
    ax3.tick_params(axis='x', rotation=45)
    
    # Plot 4: Tectonic setting analysis
    tectonic_data = {'Convergent Margins': 60, 'Cratonic Regions': 35, 'Intracontinental Rifts': 5}
    ax4.pie(tectonic_data.values(), labels=tectonic_data.keys(), autopct='%1.1f%%')
    ax4.set_title('Gold Distribution by Tectonic Setting (%)')
    
    plt.tight_layout()
    plt.savefig('gold_deposit_analysis.png', dpi=300, bbox_inches='tight')
    plt.show()
    
    # Print summary statistics
    print("GOLD DEPOSIT ANALYSIS SUMMARY")
    print("="*50)
    print(f"Total Major Deposits: {analysis_df['deposit_count'].sum()}")
    print(f"Total Reserves: {analysis_df['total_reserves'].sum()} Moz")
    print(f"Average Reserves per Deposit: {analysis_df['total_reserves'].sum() / analysis_df['deposit_count'].sum():.1f} Moz")
    print(f"Most Common Deposit Type: {analysis_df['dominant_type'].mode()[0]}")

analyze_gold_data()

