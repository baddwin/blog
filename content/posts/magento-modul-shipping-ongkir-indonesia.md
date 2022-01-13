---
title: "Modul Shipping Ongkir Indonesia untuk Magento 2"
date: 2019-07-10T00:44:41+07:00
draft: false
description: "Modul ini mengambil data dari RajaOngkir yang menyediakan API cek ongkir dan tracking jasa kurir dan ekspedisi yang ada di Indonesia"
tags: ["magento", "e-commerce", "ongkir", "web"]
---

Project pertama saya dengan Magento adalah membuat modul/_extension_
ongkir (ongkos kirim) Indonesia untuk Magento 2. Modul ini mengambil data
dari [RajaOngkir](http://rajaongkir.com) yang menyediakan API cek ongkir dan _tracking_
jasa kurir dan ekspedisi yang ada di Indonesia,
termasuk JNE, J&T, Tiki, Pos, SiCepat, dll.

Waktu itu belum ada _free extension_ (gratis) Magento untuk ongkir Indonesia.
Oleh karena itulah saya di-_hire_ untuk mengerjakannya.
Atas dasar itu pula saya mencoba membuat module yang _free_ dan _open source_.
Dengan modul ongkir ini, website Magento kita dapat menghitung
ongkir barang berdasarkan kurir yang ada di Indonesia.

Modul ini mirip dengan kode project pertama dulu, yaitu menggunakan API RajaOngkir.
Modul ini merupakan _rewrite_ yang berbeda total, karena tentunya ada NDA[^1] dengah perusahaan.
Tetapi ini bukan modul yang saya kerjakan di project tersebut.
Untuk saat ini kodenya tidak bisa saya _share_ secara ~~gratis~~ publik.
Kode yang saya _share_ di Github[^2] adalah fungsi minimum sebuah modul metode pengiriman di Magento 2.

Cuplikan kode:<!--more-->
```php
/**
     * @param RateRequest $request
     * @return \Magento\Shipping\Model\Rate\Result|bool
     */
    public function collectRates(RateRequest $request)
    {
        if (!$this->getConfigFlag('active')) {
            return false;
        }
        /** @var \Magento\Shipping\Model\Rate\Result $result */
        $result = $this->_rateResultFactory->create();
        $method = $this->generateMethods('REG', 10000, 'JNE');
        $result->append($method);
        return $result;
    }
    /**
     * @return array
     */
    public function getAllowedMethods()
    {
        return [$this->_code => $this->getConfigData('name')];
    }
    /**
     * @return \Magento\Quote\Model\Quote\Address\RateResult\Method
     */
    public function generateMethods($service, $price, $courier)
    {
        $method = $this->_rateMethodFactory->create();
        $method->setCarrier($this->_code);
        $method->setCarrierTitle($courier);
        $method->setMethod($this->_code);
        $method->setMethodTitle($service);
        $method->setPrice($price);
        $method->setCost($price);
        return $method;
    }
}
```

Kode modul tersebut merupakan hasil _generate_ dari
[sini](https://cedcommerce.com/magento-2-module-creator/shipping-module).

Contoh _live_ di website:

![screenshot ongkir](/blog/img/2019/07/ongkir.png)

Untuk mendapatkan kode yang lebih fungsional dan
disesuaikan dengan project _e-commerce_ Anda,
silakan hubungi saya melalui link akun medsos ada di atas blog. :)

Atau jika Anda merupakan perwakilan dari perusahaan, saya lebih menyarankan
untuk menggunakan API dari Shipper[^3] yang lebih lengkap dan reliabel.

[^1]: Non-disclosure Agreement
[^2]: https://github.com/FalahDev/magento2-ongkir
[^3]: https://shipper.id
