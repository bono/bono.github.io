---
layout: docsv2
title: Bundle
---

Bundle menyediakan mekanisme untuk menyatukan route-route yang terlibat dalam satu proses bisnis yang sama dalam satu modul. Pemisahan halaman-halaman berdasarkan domain masalah yang ditanganinya mempermudah untuk membuat paket-paket yang unik untuk setiap domain masalah.

Implementasi bundle paling ringkas adalah mengumpulkan satu mekanisme search, create, read, update dan delete sebuah koleksi data, dalam sebuah modul. Untuk itu, konsep controller dalam MVC yang diimplementasikan pada Bono 1 dihilangkan dan diganti dengan konsep bundle.

Bundle didefinisikan untuk menangani kumpulan dari route dengan prefix tertentu yang ditentukan melalui konfigurasi. Selain itu bundle juga memiliki middleware-middleware yang akan dijalankan sebelum route. Bahkan bundle dapat memiliki sub bundle. Konfigurasi pada `config/config.php` pada dasarnya adalah mendefinisikan bundle utama dari sebuah aplikasi.

{% highlight php linenos %}
<?php

use Bono\Middleware\ContentNegotiator;
use Bono\Middleware\MethodOverride;
use Any\Other\Middleware as AnyOtherMiddleware;
use Bono\Http\Context;

return [
    // bundle middlewares
    'middlewares' => [
        [ ContentNegotiator::class ],
        [ MethodOverride::class ],
        [ AnyOtherMiddleware::class ],
    ],
    // sub bundles
    'bundles' => [
        [ 'uri' => '/user', 'handler' => [ Bundle::class, [
            'middlewares' => [
                // ...
            ],
            'routes' => [
                [ 'uri' => '/', 'handler' => function(Context $context, $next) {
                    // do something here
                }],
                [ 'uri' => '/{id}/update', 'handler' => function(Context $context, $next) {
                    // do something here
                }],
            ]
        ]]]
    ],
    // bundle routes
    'routes' => [
        [ 'uri' => '/dashboard', 'handler' => function(Context $context, $next) {
            // do something here
        }]
    ]
];
{% endhighlight %}

### Bono Bundle

Berikut ini adalah beberapa bundle bawaan Bono.

- [Rest](Rest), Abstrak bundle yang mendukung untuk REST API

### Bundle Pihak Ketiga

Berikut ini adalah beberapa bundle dari pustaka pihak ketiga.

- [Norm Bundle](https://github.com/xinix-technology/bono-norm)

Jika anda telah membuat pustaka yang ingin dimasukkan dalam daftar di atas, hubungi maintainer Bono ;).
