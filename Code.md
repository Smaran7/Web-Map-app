# Web-Map-app
# python script for a small web app showing outlined countries on the map and marking volcanoes in the US


import folium
import pandas

def color_producer(el):
    if el <= 1500:
        return 'green'
    elif el > 1500 and el <= 3000:
        return 'orange'
    else:
        return 'red'

map = folium.Map(location=[38.8,-99.09],zoom_start=4,tiles="Mapbox Bright")

data = pandas.read_csv("Volcanoes.txt")

lat = list(data["LAT"])
lon = list(data["LON"])
elev = list(data["ELEV"])

fg=folium.FeatureGroup(name="My Map")

for i,j,k in zip(lat,lon,elev):
    fg.add_child(folium.Marker(location=[i,j], popup="Elevation: "+ str(k)+" m", icon = folium.Icon(color=color_producer(k))))

fg.add_child(folium.GeoJson(data=open('world.json', 'r', encoding= 'utf-8-sig').read(),
 style_function = lambda x: {'fillColor':'green' if x['properties']['POP2005'] < 10000000 else 'orange' if 10000000 <= x['properties']['POP2005'] < 20000000 else 'red'}))

map.add_child(fg)
map.save("Maps.html")
