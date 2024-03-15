---
title: scatterplot
---

```js
const choix_lieu = view(Inputs.select(["Arena Bercy", "Arena Champ-de-Mars", "Arena Paris Nord", 
"Arena Paris Sud", "Arena Paris Sud 1", "Arena Paris Sud 6", 
"Arena Porte de La Chapelle", "Centre aquatique", "Château de Versailles", 
"Clichy-sous-Bois", "Colline d'Elancourt", "Concorde", "Golf national", 
"Grand Palais", "Hôtel de ville-Invalides", "Invalides", "Invalides-Pont Alexandre-III", 
"Marina de Marseille", "Parc des Princes", "Paris La Défense Arena", 
"Pont Alexandre-III", "Site d'escalade", "Stade BMX", "Stade de Bordeaux", 
"Stade de France", "Stade de la Beaujoire", "Stade de Lyon", 
"Stade de Marseille", "Stade de Nice", "Stade Geoffroy-Guichard", 
"Stade nautique de Vaires-sur-Marne - Bassin eaux calmes", "Stade nautique-Bassin eau vive", 
"Stade Pierre-Mauroy", "Stade Roland-Garros", "Stade tour Eiffel", 
"Stade Yves-du-Manoir", "Trocadéro", "Vélodrome national"], {value: "Arena Bercy", label: "Choisir un site"}));
```

```js
const coordonnees_lieux = FileAttachment("coordonnees_lieux.csv").csv({typed: true});
const coordonnees_resto = FileAttachment("coordonnees_resto.csv").csv({typed: true});

```

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Leaflet Map</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
</head>
<body>
    <div id="map" style="height: 400px;"></div>



```js
const lat = coordonnees_lieux.filter(d => d.lieu === choix_lieu).map(d => d.latitude)[0];
const long = coordonnees_lieux.filter(d => d.lieu === choix_lieu).map(d => d.longitude)[0];

const map = L.map('map').setView([lat, long], 13);
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

const marker = L.marker([lat, long]).addTo(map);
marker.bindPopup(`${choix_lieu}`).openPopup();

const coord_rap = coordonnees_resto.filter(d => d.lieu === choix_lieu && d.type === "Restauration de type rapide");
 coord_rap.forEach(coord_rap => {
            L.circleMarker([coord_rap.latitude, coord_rap.longitude]).addTo(map).setStyle({color: 'red', radius: 5});
        });
const coord_trad = coordonnees_resto.filter(d => d.lieu === choix_lieu && d.type === "Restauration traditionnelle");
 coord_trad.forEach(coord_trad => {
            L.circleMarker([coord_trad.latitude, coord_trad.longitude]).addTo(map).setStyle({color: 'green', radius: 5}).bindPopup(`${coord_trad.etablissement}<br>${coord_trad.adresse}`);
        });

    

function centerMap() {
        map.setView([lat, long], 13);
}


```
<button onclick="centerMap()">Centrer la carte</button>

</body>
</html>
