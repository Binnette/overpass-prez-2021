---
theme: default
_class: lead
paginate: true
backgroundColor: #fff
marp: true
backgroundImage: url('https://marp.app/assets/hero-background.svg')
---

![bg left:35% 100%](https://wiki.openstreetmap.org/w/images/b/b5/Overpass_API_logo.svg)

# **Overpass API**

## An API to query OpenStreetMap data.


About this presentation:
👨‍💻 [User:Binnette](https://wiki.openstreetmap.org/wiki/User:Binnette)
📅 2021-12-20
🟢 License MIT

---

# **1️⃣ Intro**

---
# 🧙‍♂️ What is Overpass API*?

🔗 http://overpass-api.de/


* A __read only__ API that serves custom selected parts of OSM data
* It has 2 query languages:
  * __Overpass XML__: very verbose but trully explicit 🗣️
  * __Overpass QL__: lightweight but a bit magic ✨
* 💾 It can export data to JOSM, OSM data file, CSV, GeoJSON, ...


*️⃣ API = Application Programming Interface

---
# 👨‍🔧 Technical details

* Overpass API do not query OSM database directly. Instead it queries a clone of the main database based on a planet file (export of OSM database)
  * 🕒 It explains why data are not always up-to-date in Overpass API
* Overpass API is an ✅ OpenSource software and have many instances:
  * Main instance: 🇩🇪 https://overpass-api.de/api/interpreter
  * And [a lot of others](https://wiki.openstreetmap.org/wiki/Overpass_API#Public_Overpass_API_instances): 🇫🇷 OSM-fr, 🇨🇭 OSM-ch, 🇷🇺 OSM-ru, 🇹🇼 OSM-tw, ...
  

---
# 🗺️ About OSM data

![bg right:41% 90%](https://wiki.openstreetmap.org/w/images/c/cd/ShapeHanfelden.svg)
3 types of data ("[elements](https://wiki.openstreetmap.org/wiki/Elements)"):
* nodes: a point with lat/lon
* ways: connexion between multiples points
  * And also closed ways, areas, ...
* relations: virtual objects linking nodes and/or ways

---
# 👅 Overpass query languages

```c
/* Overpass QL: lightweight and implicit */
(
   node(51.249,7.148,51.251,7.152);
   <;
);
out meta;
```

```xml
<!-- XML: verbose but explicit -->
<union>
  <bbox-query s="51.249" w="7.148" n="51.251" e="7.152"/>
  <recurse type="up"/>
</union>
<print mode="meta"/>
```
https://overpass-turbo.eu/s/1e5Y

---
# 😅 Overpass XML 'almost' explicit...

```xml
<union>
  <bbox-query s="51.249" w="7.148" n="51.251" e="7.152"/>
  <recurse type="up"/>
</union>
<print mode="meta"/>
```

```xml
<!-- The exact same query but even more verbose... -->
<union into="_">
  <bbox-query s="51.249" w="7.148" n="51.251" e="7.152"/>
  <recurse from="_" into="_" type="up"/>
</union>
<print e="" from="_" geometry="skeleton" ids="yes" limit="" mode="meta" n="" order="id" s="" w=""/>
```

---
# 😆 Overpass XML 'full' explicit

```xml
<union into="_">
  <bbox-query s="51.249" w="7.148" n="51.251" e="7.152"/>
  <recurse from="_" into="_" type="up"/>
</union>
<print e="" from="_" geometry="skeleton" ids="yes" limit="" mode="meta" n="" order="id" s="" w=""/>
```

```xml
<!-- The exact same query but over verbose... -->
<union into="_">
  <query into="_" type="node">
    <bbox-query s="51.249" w="7.148" n="51.251" e="7.152"/>
  </query>
  <recurse from="_" into="_" type="up"/>
</union>
<print e="" from="_" geometry="skeleton" ids="yes" limit="" mode="meta" n="" order="id" s="" w=""/>
```

---
# 😎 Let convert it back to Overpass QL...

```xml
<union into="_">
  <query into="_" type="node">
    <bbox-query s="51.249" w="7.148" n="51.251" e="7.152"/>
  </query>
  <recurse from="_" into="_" type="up"/>
</union>
<print e="" from="_" geometry="skeleton" ids="yes" limit="" mode="meta" n="" order="id" s="" w=""/>
```

```c
(
   node(51.249,7.148,51.251,7.152);
   <;
);
out meta;
```

So now, you better understand the X of eXtensible in XML. 🤣

---
# 🔧 Overpass-Turbo: a tool to call Overpass API

🤩 http://overpass-turbo.eu (stable version)
😍 https://tyrasd.github.io/overpass-turbo/ (latest version)

Still the same query 😅:
 * In Overpass QL: http://overpass-turbo.eu/s/1e1F
 * In Overpass XML: http://overpass-turbo.eu/s/1e1G

Use the 'Export' button to convert to QL or XML...

---
# 📡 From Overpass-Turbo to JOSM

![bg right:13% 90%](https://josm.openstreetmap.de/browser/trunk/resources/images/preferences/remotecontrol.svg?format=raw)

* Launch JOSM and enable the "Remote control": https://josm.openstreetmap.de/wiki/Help/Preferences/RemoteControl
* Go to Overpass Turbo and run your query
* Click on Export > Data > JOSM
* Your data are loaded in JOSM 🪄

---

# 🧰 Overpass API directly in JOSM

* Open JOSM setting
* Enable 'Advanced mode'
* Click on download button
* Tab 'Download from Overpass API' is displayed

![bg right:57% 90%](https://josm.openstreetmap.de/raw-attachment/wiki/Help/Action/Download/Overpass.png)

---

# 🎲 From Overpass API to Maproulette

Why not creating MapRoulette challenges by querying wrong or incomplete OSM data with Overpass API?
https://maproulette.org/

![bg right 70%](https://wiki.openstreetmap.org/w/images/d/d6/MapRoulette-logo.svg)

---

# **2️⃣ Back to the basics**


---
# 🌎 Helloworld with Overpass

Get "drinking water" for a given "bbox":

```c
node
  [amenity=drinking_water]
  ({{bbox}});
out;
```
Try it for free!!! 💵 http://overpass-turbo.eu/s/1e1S

---
# 🗾 Instruction 'area' or how to get a specific zone

```c
area[name="France métropolitaine"]->.FRM;

area["ISO3166-2"="FR-ARA"]->.ARA; // Auvergne-Rhône-Alpes

area[name="Grenoble-Alpes Métropole"]->.LaMetro;

area[name="Grenoble"]->.Grenoble;

area["ISO3166-2"="FR-07"]->.Ardèche;

area[name="Mariac"]->.Mariac;

```

---
# 😴 Get only amenity nodes

```c
area[name="Mariac"]->.a;
node[amenity](area.a);
out geom;
```
http://overpass-turbo.eu/s/1e1U

---
# 🥱 Get amenity nodes and ways

```c
area[name="Mariac"]->.a;
nw[amenity](area.a);
out geom;
```
http://overpass-turbo.eu/s/1e1V

---
# 🤔 Get amenity nodes and ways... again

```c
area[name="Mariac"]->.a;
nw[amenity](area.a);
out geom;
```
http://overpass-turbo.eu/s/1e1V

```c
area[name="Mariac"]->.a;
(
  node[amenity](area.a);
  way[amenity](area.a);
);
out geom;
```
http://overpass-turbo.eu/s/1e1X

---
# 🤨 Get amenity nodes, ways and relations

```c
area[name="Mariac"]->.a;
nwr[amenity](area.a);
out geom;
```
http://overpass-turbo.eu/s/1e1Y

```c
area[name="Mariac"]->.a;
(
  node[amenity](area.a);
  way[amenity](area.a);
  rel[amenity](area.a);
);
out geom;
```
http://overpass-turbo.eu/s/1e1X

---

# 🎥 **DEMO TIME**

All my overpass queries:

https://wiki.openstreetmap.org/wiki/User:Binnette/OverpassQueries

---

# 💠 Appendix

* [Official Overpass API documentation](http://overpass-api.de/)
* On OSM wiki:
  * [Overpass API](https://wiki.openstreetmap.org/wiki/Overpass_API)
  * [Overpass QL](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL)
  * [Query example](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_API_by_Example)
  * [Styling Overpass Turbo with CSS](https://wiki.openstreetmap.org/wiki/Overpass_turbo/MapCSS)
  * [All my Overpass Queries](https://wiki.openstreetmap.org/wiki/User:Binnette/OverpassQueries)

---

# 💡 Any question ?

* Feel free to contact me by private message:
  * On OSM: https://www.osm.org/user/Binnette
  * On the Wiki: https://wiki.openstreetmap.org/wiki/User:Binnette
