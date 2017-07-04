---
title: "Angular.JS: Routing"
date: 2015-10-10T14:38:00+07:00
description: "AngularJs ngView dan routing"
tags: ["angularjs", "routing"]
---

Kembali membahas AngularJs, kali ini maju satu langkah, mari mempelajari routing. Pada dasarnya Angular menangani event perubahan hash pada URL dan mengarahkannya pada `ng-view`. ngView ini membutuhkan template berupa file html terpisah yang akan dimuat oleh Angular.

Berikut adalah potongan kode Javascript.

<!--more-->

```javascript
angular.module('RoutingApp', ['ngRoute'])
	.config( ['$routeProvider', function($routeProvider) {
		$routeProvider
			.when('/first', {
				templateUrl: 'first.html'
			})
			.when('/second', {
				templateUrl: 'second.html'
			})
			.otherwise({
				redirectTo: '/'
			});
 	}]);
```

Demo bisa dilihat di link berikut: <http://baddwin.github.io/demo-koding/angularjs-ng-view.html>
