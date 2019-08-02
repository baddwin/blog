---
title: "Area di Magento"
date: 2019-08-02T13:35:23+07:00
draft: false
description: "Memahami jenis-jenis area dan penggunaannya"
tags: ["magento", "php"]
---

Area di Magento merupakan sebuah komponen logis yang mengelola kode,
termasuk struktur sistem file, demi pemrosesan _request_ yang optimum[^1].
Pengelompokan area ini bermanfaat dalam menjaga kerapian struktur kode.

Area paling umum di Magento adalah `adminhtml` dan `frontend`.
_adminhtml_ merupakan<!--more--> area kode yang mengontrol proses yang terjadi pada admin.
Sedangkan _frontend_ adalah area _store front_ untuk _consumer facing_, seperti view product, proses checkout, registrasi user, dll.

Nama-nama area yang disediakan Magento antara lain:

1. _adminhtml_: area untuk halaman admin
2. _frontend_: halaman depan consumer facing
3. _base_: fallback area jika tidak ada kode di adminhtml atau frontend
4. _crontab_: khusus untuk cron job proses
5. _webapi_rest_: menangani proses yang berhubungan dengan request REST API
6. _webapi_soap_: menangani proses yang berhubungan dengan request SOAP API

Contoh use case area ini adalah dalam routing.
Untuk routing frontend, sudah dibahas pada [tulisan sebelumnya][ref1].
Sedangkan untuk routing adminhtml, misalnya akan membuat page di admin
dengan url `/admin/blog/post/new`.
Maka routes.xml berisi:

```
<?xml version="1.0" ?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
    <router id="adminhtml">
        <route id="blog" frontName="blog">
            <module name="Vendor_NamaModul"/>
        </route>
    </router>
</config>
```
Di mana path `blog` ditentukan di tag route dengan key _frontName_.
Perbedaannya dengan route frontend adalah, di area adminhtml selalu didahului dengan path `/admin`.

Untuk controllernya, letak file controller di dalam folder `Adminhtml`.
Selebihnya mengikuti format struktur yang telah ditentukan seperti pada standard route. Maka struktur controller seperti ini:
```
├── Adminhtml                                                                   
│   └── Blog                                                                                                                        
│       └── Post                                                            
│           └── New.php                                                        
```

Referensi resmi dari [halaman dokumentasi Magento][ref2].

[^1]: kata guru bahasa Indonesia dulu, akhiran _-mum_ lebih baku daripada _-mal_

[ref1]: {{< ref "routing-di-magento" >}}
[ref2]: https://devdocs.magento.com/guides/v2.3/architecture/archi_perspectives/components/modules/mod_and_areas.html