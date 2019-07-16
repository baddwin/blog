---
title: "Modul Shipping Ongkir Indonesia untuk Magento 2"
date: 2019-07-10T00:44:41+07:00
draft: false
description: "Modul ini mengambil data dari RajaOngkir yang menyediakan API cek ongkir dan tracking jasa kurir dan ekspedisi yang ada di Indonesia"
tags: ["magento", "e-commerce", "ongkir", "web"]
---

Selama mempelajari[^1] Magento, saya sudah membuat modul/_extension_
ongkir (ongkos kirim) Indonesia untuk Magento 2. Modul ini mengambil data
dari [RajaOngkir](http://rajaongkir.com) yang menyediakan API cek ongkir dan _tracking_
jasa kurir dan ekspedisi yang ada di Indonesia,
termasuk JNE, J&T, Tiki, Pos, SiCepat, dll.
Dengan modul ongkir ini, website Magento kita dapat menghitung
ongkir barang berdasarkan kurir yang dipilih.

Modul ini mirip dengan kode awal project yang saya kerjakan di tempat kerja saya --dulu.
Kodenya tidak bisa saya _share_ secara ~~gratis~~ publik.
Tapi cuplikan kodenya sudah saya _share_ di Github[^2].

Cuplikan:<!--more-->
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

![screenshot ongkir](/img/2019/07/ongkir.png)

Untuk mendapatkan kode yang lebih fungsional dan
disesuaikan dengan project _e-commerce_ yang Anda bikin,
silakan hubungi saya. (link akun medsos ada di atas blog) :)

[^1]: selama bekerja
[^2]: https://github.com/baddwin/magento2-ongkir
