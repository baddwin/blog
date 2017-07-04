---
title: "Chart.js: menggambar diagram dengan JavaSript"
date: 2015-07-24T13:54:32+07:00
description: "Chart.Js untuk menggambar diagram"
tags: ["chartjs", "javascript"]
---

Kali ini saya share hasil eksplorasi atas [Chart.js](http://www.chartjs.org/ ""), skrip JavaScript untuk memvisualisasikan data dalam berbagai macam bentuk diagram. Di sini saya menggunakan diagram garis.
<!--more-->

```html
<canvas id="diagram" width="800" height="400"></canvas>

<div id="chart-legend"></div>
        
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/1.0.2/Chart.min.js"></script>
<script type="text/javascript">
// Chart data & properties
var data = {
    labels: ["2015-07-13","2015-07-14","2015-07-15","2015-07-16","2015-07-17","2015-07-18","2015-07-19","2015-07-20","2015-07-21","2015-07-22"],
    datasets: [
        {
            label: "Data Statistik",
            fillColor: "rgba(0,0,0,0)",
            strokeColor: "rgba(66, 139, 202,1)",
            pointColor: "rgba(86, 66, 230, 1)",
            pointStrokeColor: "#fff",
            pointHighlightFill: "#fff",
            pointHighlightStroke: "rgba(66, 139, 202, 0.5)",
            data: ["64","69","62","56","39","44","44","49","48","56"]
        },
        {
            label: "Rata-rata data",
            fillColor: "rgba(0,0,0,0)",
            strokeColor: "rgba(202, 77, 66, 1)",
            pointColor: "rgba(202, 0, 0, 1)",
            pointStrokeColor: "#fff",
            pointHighlightFill: "#fff",
            pointHighlightStroke: "#FAAA78",
            data: ["64","66.5","65","62.75","58","55.67","54","53.38","52.78","53.1"]
        }
    ]
}

var ctx = document.getElementById("diagram").getContext("2d");
var diagramBaru = new Chart(ctx).Line(data, {
        bezierCurve: false,
        scaleShowVerticalLines: false,
        responsive: true
});
document.getElementById('chart-legend').innerHTML = diagramBaru.generateLegend();
</script>
```

Demo bisa dilihat di link berikut: <http://baddwin.github.io/demo-koding/angularjs-chartjs.html>

Sumber ide:

1. <http://www.chartjs.org/docs/>
2. <http://jsfiddle.net/vrwjfg9z/>
