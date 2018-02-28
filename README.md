# geographic-information-system
import folium
import pandas


data=pandas.read_csv("convertcsv.csv.txt")
data1=pandas.read_csv("Volcanoes.txt")
lon=list(data["longitude"])
lat=list(data["latitude"])
lon1=list(data1["LON"])
lat1=list(data1["LAT"])
elev=list(data1["ELEV"])
elev1=list(data["altitude"])

def color1(elevation):
    if elevation<1000:
        return 'green'
    elif elevation>1000<2000:
        return 'orange'
    else:
        return'red'

map=folium.Map(location=[28.271367, 76.719085],zoom_start=6,tiles="Mapbox Bright")
fg=folium.FeatureGroup(name="volcanoes")
fg1=folium.FeatureGroup(name="population")

for lt,ln,el in zip(lat,lon,elev1):
    fg.add_child(folium.Marker(location=[lt,ln],popup=str(el)+" m",icon=folium.Icon(color=color1(el))))

for lt,ln,el in zip(lat1,lon1,elev):
    fg.add_child(folium.Marker(location=[lt,ln],popup=str(el)+" m",icon=folium.Icon(color=color1(el))))

fg1.add_child(folium.GeoJson(data=open('world.json','r',encoding='utf-8-sig').read(),
style_function=lambda x: {'fillColor':'green' if x['properties']['POP2005'] < 10000000
else 'yellow' if 10000000<= x['properties']['POP2005'] < 20000000 else 'red'}))
map.add_child(fg)
map.add_child(fg1)

map.add_child(folium.LayerControl())
map.save("Map1.html")
