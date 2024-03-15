---
title: podium
---

```js
import * as echarts from "npm:echarts";
```

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
const podium = FileAttachment("type_resto_podium.csv").csv({typed: true});
```

```js
const podium_type = echarts.init(display(html`<div style="width: 600px; height:400px;"></div>`));

var option;

const labelOption = {
  show: true,
  position: 'top',
  formatter: '{name|{a}}',
  fontSize: 16,
  rich: {
    name: {}
  }
};

option = {
  xAxis: {
    type: 'category',
    data: [1],
    show: false
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      data: podium.filter(d => d.lieu === choix_lieu).map(d => d.pct).splice(0, 1),
      name: podium.filter(d => d.lieu === choix_lieu).map(d => d.type).splice(0, 1),
      type: 'bar',
      label: labelOption
    },
    {
      data: podium.filter(d => d.lieu === choix_lieu).map(d => d.pct).splice(1, 1),
      name: podium.filter(d => d.lieu === choix_lieu).map(d => d.type).splice(1, 1),
      type: 'bar',
      label: labelOption
    },
    {
      data: podium.filter(d => d.lieu === choix_lieu).map(d => d.pct).splice(2, 1),
      name: podium.filter(d => d.lieu === choix_lieu).map(d => d.type).splice(2, 1),
      type: 'bar',
      label: labelOption
    }
  ]
};

option && podium_type.setOption(option);

```
