---
title: "Coding pertama: Angular.Js penyortir tabel"
date: 2015-07-23T19:51:13+07:00
description: "Angular.Js penyortir tabel"
tags: ["angularjs", "javascript"]
---

Saya mau share hasil koding pertama belajar AngularJs. Berikut *snippet* kodenya.
<!--more-->

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Hafal Quran Android App</title>
<meta name="viewport" content="width=device-width, initial-scale=1">

<script type="text/javascript" src="script/angular.min.js"></script>
</head>
<body ng-app="statistikApp">
<section ng-controller="urutkanTabel">
    <h2 class="table">Statistik Hafal Quran</h2>
    <h3 class="table"><a href="https://play.google.com/store/apps/details?id=id.web.amzone.hafalquran" target="_blank">Android App</a></h3>
    <p style="text-align:center;"><?php echo $Tanggal;?></p>
    <table>
    <thead>
        <tr>
            <th>No.</th>
            <th><a href="" ng-click="reverse=!reverse;order('a', reverse)">Kode</a></th>
            <th><a href="" ng-click="reverse=false;order('b', false)">First</a> (<a href="" ng-click="order('-b',false)">^</a>)</th>
            <th><a href="" ng-click="reverse=false;order('c', false)">Last</a> (<a href="" ng-click="order('-c',false)">^</a>)</th>
            <th><a href="" ng-click="reverse=!reverse;order('d', reverse)">Level</a></th>
            <th><a href="" ng-click="reverse=!reverse;order('e', reverse)">Play</a></th>
            <th><a href="" ng-click="reverse=!reverse;order('f', reverse)">Device</a></th>
        </tr>
    </thead>
    <tbody>

        <tr ng-repeat="stat in stats | orderBy:last:reverse">
            <td>{{$index+1}}</td>
            <td>{{stat.a}}</td>
            <td>{{stat.b}}</td>
            <td>{{stat.c}}</td>
            <td>{{stat.d}}</td>
            <td>{{stat.e}}</td>
            <td>{{stat.f}}</td>
        </tr>

    </tbody>
    </table>

</section>

<script type="text/javascript">
var app = angular.module('statistikApp', []);
app.controller('urutkanTabel', ['$scope', '$filter', function($scope, $filter) {
  var orderBy = $filter('orderBy');
  $scope.stats = <?php print json_encode($stats);?>;
  $scope.order = function(predicate, reverse) {
    $scope.stats = orderBy($scope.stats, predicate, reverse);
  };
  $scope.order('-c',false);
}]);
</script>

</body>
</html>
```

Demo bisa dilihat di link berikut: <http://baddwin.github.io/demo-koding/angularjs-sortir-tabel.html>

Ide dari beberapa sumber berikut:

1. <http://www.mograblog.com/2013/06/angularjs-smart-table.html>
2. <http://jsfiddle.net/patxy/D2FsZ/light/>
3. <https://gist.github.com/simonbingham/3858121>
4. <http://stackoverflow.com/questions/18789973/sortable-table-columns-with-angularjs>
5. <http://www.w3schools.com/angular/angular_tables.asp>
6. <https://docs.angularjs.org/api/ng/filter/orderBy>
