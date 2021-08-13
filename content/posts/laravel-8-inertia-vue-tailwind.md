---
title: "Mencoba Laravel 8 Breeze dengan Inertia, Vue, dan Tailwind"
date: 2021-08-13T23:48:54+07:00
draft: false
description: "Laravel Breeze also offers an Inertia.js frontend implementation powered by Vue or React."
tags: ["laravel", "php", "web"]
---

Beberapa waktu terakhir saya lebih fokus dengan Magento.
Padahal sebelum itu saya berangkat dari _framework_ Laravel.
Cukup aneh jika blog yang saya khususkan untuk kode ini hanya berisi satu
pos tentang Laravel. Itu pun tidak terlalu berfaedah.

Baru-baru ini saya mengoprek Laravel 8 dengan Inertia.js dan Vue.js serta Tailwind CSS.
Semuanya dibundel oleh starter kit Breeze[^1].
Dalam kurun waktu 2-3 tahun belakangan ternyata Laravel merilis banyak _tools_
dengan istilah-istilah baru.
Saya merasa sangat ketinggalan karena tidak terlalu mengikuti perkembangannya.

Mengenai Breeze, di laman resminya dijelaskan,

> Laravel Breeze also offers an Inertia.js frontend implementation powered by Vue or React.

Selain Vue, Breeze menawarkan solusi menggunakan React. Untuk instalasi pada Laravel,
cukup menjalankan komando:

    php artisan breeze:install vue
    
Perintah itu akan menginstall dan membuat _boilerplate frontend_ Inertia dan Vue.
Sesuaikan jika lebih memilih React.

Project yang saya oprek ini merupakan sebuah _order management system_ (OMS) sederhana.
Dengar OMS, saya otomatis mengasosiasikannya dengan Magento.
Oleh karena itu, secara arsitektur oprekan ini juga sangat terinspirasi oleh Magento.
Yang paling kentara adalah tabel _cart_ atau di Magento istilahnya _quote_ yang digunakan
sebagai _cache_ sebelum transaksi disimpan yang akhirnya masuk ke tabel orderan.

Di sini saya beri nama `TransactionBag` untuk model _cart_.

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class TransactionBag extends Model
{
    use HasFactory;

    protected $fillable = [
        'user_id',
        'total_payment',
        'created_at',
        'updated_at',
    ];

    public function items()
    {
        return $this->belongsToMany(TransactionItem::class, 'transaction_bag_items', 'bag_id', 'item_id')
            ->withPivot('qty', 'total_price');
    }

    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```

![screenshot dashboard admin](/img/2021/08/screen_000953.png)

![screenshot dashboard admin](/img/2021/08/screen_001025.png)

Kode project lengkap sudah saya unggah ke Github[^2].

*P.S. sebenarnya ini hasil pengerjaan tes teknikal sebuah job yang saya lamar. :)*


[^1]: [Laravel Breeze](https://laravel.com/docs/8.x/starter-kits#laravel-breeze)
[^2]: https://github.com/baddwin/laravue
