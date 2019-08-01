---
title: "Routing di Magento"
date: 2019-08-02T00:03:13+07:00
draft: false
description: "Bagaimana Magento menangani routes"
tags: ["magento", "php"]
---

Routing di Magento ditentukan per-modul. Ada beberapa jenis _routing_,
antara lain _urlrewrite, standard, cms, dan default_.
Routing juga dibatasi oleh area (soal area akan dibahas pada tulisan berikutnya).
Area yang disediakan oleh Magento secara default adalah `adminhtml` dan `frontend`.

Tulisan ini akan membahas standard routing di area frontend.
Routing jenis ini bisa dibuat dengan layout xml, atau dengan aturan <!--more-->
`<baseUrl>/[<storeCode>]/<frontName>/<controller>/<action>`.

Hal pertama yang harus dibuat adalah file `etc/frontend/routes.xml`:
```
<?xml version="1.0" ?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
    <router id="%routerId%">
        <route id="%routeId%" frontName="%frontName%">
            <module name="%moduleName%"/>
        </route>
    </router>
</config>
```
`%routerId%` diganti `standard` karena ini adalah standard routing.
`%frontName%` disesuaikan dengan `<frontName>` pada aturan.
Nilainya biasanya sama dengan `%routeId%`.
`%moduleName%` disesuaikan dengan modul yang mendefinisikannya, dengan format `Vendor_NamaModul`.

Kemudian buat controller sesuai aturan, yang ditaruh di folder Controller.
Contohnya, url yang akan dibuat adalah `/blog/show/post`.
Maka controller dibuat di _app/code/Vendor/NamaModul/Controller/Show/Post.php_.
Perhatikan path `blog` tidak terdapat di URI file controller,
karena ia adalah front name yang sudah disebut di routes.xml.

File Post.php harus meng-_extend_ class _\Magento\Framework\App\Action\Action_.
Maka contoh controller sebagai berikut:
```
<?php
namespace Vendor\NamaModul\Controller\Show;

class Post extends \Magento\Framework\App\Action\Action
{
    function execute() {
        return $result;
    }
}
```
Function `execute()` adalah function utama dari controller tersebut.
`$result` adalah hasil return yang diharapkan, bisa berupa page, redirect, layout, raw atau JSON.
Akan dibahas pada tulisan mendatang. _Stay tuned!_