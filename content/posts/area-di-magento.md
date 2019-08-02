---
title: "Area di Magento"
date: 2019-08-02T13:35:23+07:00
draft: true
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
1. adminhtml: area untuk halaman admin
2. frontend: halaman depan consumer facing
3. base: fallback area jika tidak ada kode di adminhtml atau frontend
4. crontab: khusus untuk cron job proses
5. webapi_rest: menangani proses yang berhubungan dengan request REST API
6. webapi_soap: menangani proses yang berhubungan dengan request SOAP API

Contoh use case area ini adalah dalam routing.
Untuk routing frontend, sudah dibahas pada {{< ref "routing-di-magento" >}}.

Referensi resmi dari [halaman dokumentasi Magento][2].

[^1]: kata guru bahasa Indonesia dulu, akhiran _-mum_ lebih baku daripada _-mal_

[2]: https://devdocs.magento.com/guides/v2.3/architecture/archi_perspectives/components/modules/mod_and_areas.html