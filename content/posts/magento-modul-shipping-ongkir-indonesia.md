---
title: "Modul Shipping Ongkir Indonesia untuk Magento 2"
date: 2019-07-10T00:44:41+07:00
draft: false
description: ""
tags: ["magento", "e-commerce", "ongkir", "web"]
---

Selama mempelajari Magento[^1], saya sudah membuat modul/_extension_
ongkir (ongkos kirim) Indonesia untuk Magento 2. Modul ini mengambil data
dari [RajaOngkir](http://rajaongkir.com) yang menyediakan API cek ongkir dan tracking
jasa kurir dan ekspedisi yang ada di Indonesia.

Modul ini adalah project yang saya kerjakan di tempat kerja saya --dulu.
Kodenya tidak saya _share_ secara ~~gratis~~ publik.
Tapi sudah saya _share_ cuplikannya di Github[^2].

Cuplikan:
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

<!-- more -->
Kode modul tersebut merupakan hasil _generate_ dari
[sini](https://cedcommerce.com/magento-2-module-creator/shipping-module).
Kalau ada yang memerlukan kode yang lebih lengkap,
menyesuaikan project _e-commerce_ yang Anda bikin,
silakan boleh hubungi saya. Hehe

[^1]: selama bekerja
[^2]: https://github.com/baddwin/magento2-ongkir