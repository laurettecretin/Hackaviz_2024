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

// Définir les catégories
const categories = podium.filter(d => d.lieu === choix_lieu).map(d => d.type)

// Définir les données de la série
const data = podium.filter(d => d.lieu === choix_lieu).map(d => d.pct)

// Définir les options du graphique
option = {
    color: ['#8B0000'],
    //, ['#C0C0C0','#FFD700', '#CD7F32'],
    xAxis: {
        type: 'category',
        data: podium.filter(d => d.lieu === choix_lieu).map(d => d.type),
        show: false
    },
    yAxis: {
        type: 'value',
        axisLabel: { 
            formatter: '{value} %' 
        }
    },
    series: [{
        name: 'pct',
        type: 'bar',
        data: data,
        label: { 
            show: true, 
            position: 'top', 
            formatter: '{b}' 
        }
    }]
};

// Appliquer les options au graphique
podium_type.setOption(option) 
```

