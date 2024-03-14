---
title: Test apexcharts
toc: true
---



```js
import ApexCharts from 'npm:apexcharts'
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
const podium = FileAttachment("test_podium.csv").csv({typed: true});
```

```js
var options = {
    series: [{ data: podium.filter(d => d.lieu === choix_lieu).map(d => (d.place, d.pct)),
    name :  podium.filter(d => d.lieu === choix_lieu).map(d => d.type)}],
    chart: {
          type: 'bar',
          height: 350
        },
    plotOptions: {
          bar: {
            borderRadius: 4,
            horizontal: false
          }
        },
    dataLabels: {
          enabled: false
        },
    xaxis: {show : false},
    yaxis: {show : false},
    annotations:{
      texts:[{
        y : podium.filter(d => d.lieu === choix_lieu).map(d => d.pct),
        x : podium.filter(d => d.lieu === choix_lieu).map(d => d.place),
        text : podium.filter(d => d.lieu === choix_lieu).map(d => d.type)
      }]
    }
    };

var graph_podium = new ApexCharts(document.querySelector("#graph_podium"), options);
        graph_podium.render();
```
<div id="graph_podium"></div>

```js
const icones_disciplines = FileAttachment("icones_disciplines.csv").csv({typed: true});
```
<!---
```js
var options = {
          series: [{
             data: icones_disciplines.filter(d => d.lieu === choix_lieu).map(d => (d.y, d.x))
          } 
            ],
          chart: {
          height: 350,
          type: 'scatter',
        },
        dataLabels: {
          enabled: false
        },
        colors: ["#008FFB"],
        fill: {
  type: 'image',
  image: {
    src: icones_disciplines.filter(d => d.lieu === choix_lieu).map(d => (d.image)),
    width: undefined,  // optional
    height: undefined  //optional
  }
}
        };

        var chart = new ApexCharts(document.querySelector("#chart"), options);
        chart.render();
      
      
    
```
<div id="chart"></div>
--->