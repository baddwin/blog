---
title: "Routing di Magento (Lanjutan)"
date: 2019-08-07T12:32:36+07:00
draft: true
description: "Macam-macam return type yang dibuat oleh routing Magento"
tags: ["magento", "php"]
---

Seperti janji saya di [tulisan tentang routing][ref1] sebelumnya, kali ini adalah lanjutannya.
Routing di Magento di-_handle_ oleh satu _controller_ untuk tiap _route_, di method `execute`.
_Return_ dari method itu antara lain: _page, json, raw, layout, redirect dan forward_.

### Page result
Route ini me-_return_ halaman HTML.
Class yang digunakan adalah `\Magento\Framework\View\Result\Page`.

Contoh kode:
```
<?php
namespace Modul\NamaVendor\Controller\Index;

class Index extends \Magento\Framework\App\Action\Action
{
    protected $_pageFactory;
    public function __construct(
        \Magento\Framework\App\Action\Context $context,
        \Magento\Framework\View\Result\PageFactory $pageFactory) {
        $this->_pageFactory = $pageFactory;
        return parent::__construct($context);
    }

    public function execute()
    {
        return $this->_pageFactory->create();
    }
}
```
Controller tersebut akan membuat halaman di `/blog/index/index`
atau cukup disingkat dengan `/blog`.
Tentunya frontName 'blog' sudah ditentukan di `etc/frontend/routes.xml`,
seperti sudah dijelaskan pada [tulisan sebelumnya][ref1].

Selanjutnya perlu membuat layout xml di `/Vendor/NamaModul/etc/frontend/layout/blog_index_index.xml`.
```
```

Kemudian, buat _block_ dan template untuk menentukan template yang dipakai oleh halaman.
Seperti ditetapkan di layout xml di atas, maka dibuat 2 file baru:
```
// Block/Blog/Index.php

```
```
// view/frontend/web/templates/blog/index.phtml
```

### JSON result

### RAW result

### Layout result

### Redirect result

### Forward result


[ref1]: {{< ref "routing-di-magento" >}}