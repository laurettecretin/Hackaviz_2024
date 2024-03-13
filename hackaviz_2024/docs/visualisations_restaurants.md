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