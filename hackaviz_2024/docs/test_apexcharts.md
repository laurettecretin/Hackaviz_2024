---
title: Test apexcharts
toc: true
---



```js
import ApexCharts from 'npm:apexcharts'
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
const podium = FileAttachment("test_podium.csv").csv({typed: true});
const type_resto_scatter = FileAttachment("type_resto_scatter.csv").csv({typed: true});
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
    xaxis: {show : false, type : "category"},
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

```js
const myChart = echarts.init(display(html`<div style="width: 600px; height:400px;"></div>`));

var option;

option = {
  xAxis: {},
  yAxis: {},
  series: [
    {
      symbolSize: 20,
      data: [
        [10.0, 8.04, 1],
        [8.07, 6.95, 1],
        [13.0, 7.58, 1],
        [9.05, 8.81, 2],
        [11.0, 8.33, 2],
        [14.0, 7.66, 2],
        [13.4, 6.81, 3],
        [10.0, 6.33, 3],
        [14.0, 8.96, 3],
        [12.5, 6.82, 4],
        [9.15, 7.2, 4],
        [11.5, 7.2, 4],
        [3.03, 4.23, 5],
        [12.2, 7.83, 5],
        [2.02, 4.47, 5],
        [1.05, 3.33, 5],
        [4.05, 4.96, 6],
        [6.03, 7.24, 6],
        [12.0, 6.26, 6],
        [12.0, 8.84, 7],
        [7.08, 5.82, 7],
        [5.02, 5.68, 7]
      ],
      type: 'scatter'
    },{
    symbolSize: 20,
    color: '#BEBEBE',
    data: [[6, 2, 10]],
    type : 'effectScatter' }
  ],
};
console.log("option:", option)
myChart.setOption(option) 
```
<div id="main">
${myChart}
</div>

