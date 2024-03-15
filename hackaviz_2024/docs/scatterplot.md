---
title: scatterplot
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
const type_resto_scatter = FileAttachment("type_resto_scatter.csv").csv({typed: true});
```

```js
const scatterplot = echarts.init(display(html`<div style="width: 600px; height:400px;"></div>`));

var option;

option = {
  grid: {
        left: '10%', 
        right: '20%', 
        bottom: '15%', 
        containLabel: true 
    },
    xAxis: {
    name: 'Part de \nrestauration \ntraditionnelle',
    min: 20, max: 80,
    axisLine: { 
            symbol: ['none', 'arrow'], 
            symbolSize: [8, 16], 
            lineStyle: {
                color: '#333' 
                }
            }
    },
  yAxis: {
    name: 'Part de \nrestauration rapide',
    min: 20, max: 80,
    axisLine: { 
            symbol: ['none', 'arrow'], 
            symbolSize: [8, 16], 
            lineStyle: {
                color: '#333' 
                }
            }
    },
  series: [
    {
      symbolSize: 8,
    color: '#8B0000',
      data: type_resto_scatter.filter(d => d.lieu !== choix_lieu).map( d => [d.restauration_traditionnelle, d.restauration_de_type_rapide]),
      type: 'scatter'
    },{
    symbolSize: 20,
    color: '#2E8B57',
    data: type_resto_scatter.filter(d => d.lieu === choix_lieu).map( d => [d.restauration_traditionnelle, d.restauration_de_type_rapide]),
    type : 'effectScatter' }
  ],
};
scatterplot.setOption(option) 
```
