---
theme: dashboard
title: Restaurants
toc: false
---

# Restaurants



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


<div class="grid grid-cols-1">
  <div class="card">
    ${choix_lieu}
  </div>
</div>

<!-- Load and transform the data -->

```js
const nb_resto_par_lieux = FileAttachment("nb_resto_par_lieux.csv").csv({typed: true});
const podium = FileAttachment("test_podium.csv").csv({typed: true});
const disciplines_par_lieux = FileAttachment("disciplines_par_lieux.csv").csv({typed: true});
const coordonnees_resto = FileAttachment("coordonnees_resto.csv").csv({typed: true});
const coordonnees_lieux = FileAttachment("coordonnees_lieux.csv").csv({typed: true});
const bbox_lieux = FileAttachment("bbox_lieux.csv").csv({typed: true});
```

<div class="grid grid-cols-4">
  <div class="card">
    <h2>Nombre de resto dans le coin</h2>
    <span class="big">${nb_resto_par_lieux.filter(d => d.lieu === choix_lieu).map(d => d.n)}</span>
  </div>
  <div class="card">
    <h2>Le plus près s'appelle...</h2>
    <span class="big">${nb_resto_par_lieux.filter(d => d.lieu === choix_lieu).map(d => d.nom_plus_pres)}</span>
  </div><div class="card">
    <h2>Le resto le plus près est à...</h2>
    <span class="big">${nb_resto_par_lieux.filter(d => d.lieu === choix_lieu).map(d => d.plus_pres)}</span>
  </div><div class="card">
    <h2>Le resto le plus loin est à...</h2>
    <span class="big">${nb_resto_par_lieux.filter(d => d.lieu === choix_lieu).map(d => d.plus_loin)}</span>
  </div>
</div>


```js
const graph_podium = Plot.plot({
    axis: null,
    marks: [
    Plot.barY(podium, {
      x: "place",
      y: "pct",
      fill: "#d38f9e",
      filter: d => d.lieu === choix_lieu
    }),
    Plot.text(podium, {
      x : "place",
      y : "pct",
      text : "type",
      dy : -20,
      fill: "#bd586e",
      filter: d => d.lieu === choix_lieu
    })
  ]
})
```

<div class="grid grid-cols-1">
  <div class="card">
    ${graph_podium}
  </div>
</div>

```js
const graph_disciplines = Plot.plot({
  y: {
    axis: null,
    domain: disciplines_par_lieux.filter(d => d.lieu === choix_lieu).sort(d => -d.n).map(d => d.discipline)
  },
  marks: [
    Plot.barX(disciplines_par_lieux,{
      x : "n",
      y : "discipline",
      filter : d => d.lieu === choix_lieu
    }),
    Plot.axisY({textAnchor: "start", fill: "white", dx: 14})
  ]
})
```
<div class="grid grid-cols-1">
  <div class="card">
    ${graph_disciplines}
  </div>
</div>


```js
const carte_resto = Plot.plot({
  projection: "albers",
  marks: [
    Plot.dot(
      coordonnees_resto,{
        filter: d => d.lieu === choix_lieu
      },
      Plot.hexbin(
        { r: "count", fill: "count" },
        { x: "longitude", y: "latitude" }
      )
    )
  ],
  height: 500,
  width: 800,
  margin: 50,
  r: { range: [0, 15] },
  x : {domain : [bbox_lieux.filter(d => d.lieu === choix_lieu).map(d => d.xmin), bbox_lieux.filter(d => d.lieu === choix_lieu).map(d => d.xmax)]},
  y : {domain : [bbox_lieux.filter(d => d.lieu === choix_lieu).map(d => d.ymin), bbox_lieux.filter(d => d.lieu === choix_lieu).map(d => d.ymax)]},
  color: {
    legend: true,
    label: "Nombre de restaurants à proximite:",
    scheme: "cool"
  }
})
```

<div class="grid grid-cols-1">
  <div class="card">
    ${carte_resto}
  </div>
</div>

```js
const carte_resto2 = Plot.plot({
  marks: [
    Plot.dot(coordonnees_resto, {
      x: "longitude",
      y: "latitude",
      filter : d => d.lieu === choix_lieu,
      fill: "type", 
      opacity: 0.7 // Decrease opacity (0 = transparent, 1 = opaque)
    }),
    Plot.dot(coordonnees_lieux, {
      x: "longitude",
      y: "latitude",
      filter : d => d.lieu === choix_lieu,
      opacity: 1 // Decrease opacity (0 = transparent, 1 = opaque)
    })
  ],
  x : {domain : [bbox_lieux.filter(d => d.lieu === choix_lieu).map(d => d.xmin), bbox_lieux.filter(d => d.lieu === choix_lieu).map(d => d.xmax)]},
  y : {domain : [bbox_lieux.filter(d => d.lieu === choix_lieu).map(d => d.ymin), bbox_lieux.filter(d => d.lieu === choix_lieu).map(d => d.ymax)]},
  color: { legend: true }, // Include a legend for the fill color
  height: 500,
  width: 800,
  margin: 50
})
```

<div class="grid grid-cols-1">
  <div class="card">
    ${carte_resto2}
  </div>
</div>