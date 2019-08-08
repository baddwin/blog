---
title: "Routing di Magento (Lanjutan)"
date: 2019-08-07T12:32:36+07:00
draft: false
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
<?xml version="1.0"?>
<layout xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/layout_generic.xsd">
    <container name="root">
        <block class="Vendor\NamaModul\Block\Ajax\Brand" name="order_brand" template="Vendor_NamaModul::blog/index.phtml" cacheable="false"/>
    </container>
</layout>
```

Kemudian, buat _block_[^1] dan template untuk menentukan template yang dipakai oleh halaman.
Seperti ditetapkan di layout xml di atas, maka dibuat 2 file baru:
```php
// Block/Blog/Index.php
<?php
namespace Vendor\NamaModul\Block\Blog;

class Index extends \Magento\Framework\View\Element\Template
{
    public function __construct(\Magento\Framework\View\Element\Template\Context $context)
    {
        parent::__construct($context);
    }

    public function _prepareLayout()
    {
        return parent::_prepareLayout();
    }

    /**
     * Contoh data diambil dari model
     * @return \Vendor\NamaModul\Model\Blog
     */
    public function getBlogList()
    {
        $blogData = $this->blogModel->getBlog();
        return $blogData;
    }
}

```
Pada contoh di atas, data blog mengambil dari model.
Tentang model akan dijelaskan di tulisan mendatang.
```
// view/frontend/web/templates/blog/index.phtml
<?php foreach($block->getBlogList() as $blog) : ?>
<h1><?= $blog->getTitle() ?></h1>

<p><?= $blog->getExcerpt() ?></p>
<?php endforeach; ?>
```

### JSON result
Result ini digunakan untuk membuat response JSON yang lebih general selain REST.

```php
public function __construct(
    //...
    Magento\Framework\Controller\Result\JsonFactory $jsonResultFactory,
    //...        
)
{
    $this->jsonResultFactory = $jsonResultFactory;
}

public function execute()
{
    $result = $this->jsonResultFactory();

    $o = new stdClass;              
    $o->foo = 'bar';
    $result->setData($o);
    return $result;              
}
```

### RAW result
Result ini membuat return yang benar-benar _raw_,
jadi sangat fleksibel untuk membuat response yang tidak biasa.

```php
public function __construct(
    //...
    Magento\Framework\Controller\Result $rawResultFactory ,
    //...        
)
{
    $this->rawResultFactory = $rawResultFactory;
}

public function execute(
)
{
    //...
    $result = $this->rawResultFactory->create();
    $result->setHeader('Content-Type', 'text/xml');
    $result->setContents('<root><science></science></root>);
    return $result;
}
```

### Layout result
Layout result sebenarnya raw result, yang berpadu dengan page result.
Hanya saja layout result ini tidak membuat seluruh halaman.
Jadi yang di-return adalah satu template phtml saja,
tidak termasuk header dan footer atau static files (JS, CSS).
Dan class yang digunakan adalah sama seperti raw result.
```
// controller
...
    public function __construct(
        \Magento\Framework\App\Action\Context $context,
        \Magento\Framework\Controller\ResultFactory $result,
        ...
    ) {
        ...
        $this->resultFactory = $result;
        return parent::__construct($context);
    }

    public function execute()
    {
        ...
        return $this->resultFactory->create(ResultFactory::TYPE_LAYOUT);
    }
}
```

Selayaknya page result, perlu dibuat layout xml dan template phtml juga selain controller tersebut.

### Redirect result
Result ini akan mengalihkan ke URL yang ditentukan.

```php
public function __construct(
    //...
    Magento\Framework\Controller\Result\RedirectFactory $resultRedirectFactory
    //...        
)
{
    $this->resultRedirectFactory = $resultRedirectFactory;
}

public function execute()
{
    $result = $this->resultRedirectFactory->create();
    $result->setPath('*/*/index');
    return $result;
}
```

### Forward result
Result ini akan meneruskan ke URL yang telah ditentukan.

```php
public function __construct(
    //...
    Magento\Framework\Controller\Result\ForwardFactory $resultForwardFactory
    //...        
)
{
    $this->resultForwardFactory = $resultForwardFactory;
}

public function execute()
{
    $result = $this->resultForwardFactory->create();
    $result->forward('noroute');    
    return $result;
}
```

Inspirasi dari [Alan Storm][ref2].


[^1]: block adalah class khusus yang menjadi jembatan atau penghubung antara layout xml dan template phtml, semua logic yang dipakai oleh template berada di sini yang bisa diakses dengan variable `$block`

[ref1]: {{< ref "routing-di-magento" >}}

[ref2]: https://alanstorm.com/magento-2-controller-result-objects/