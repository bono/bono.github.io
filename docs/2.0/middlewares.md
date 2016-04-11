---
layout: docsv2
title: Middleware
---

Middleware menyediakan mekanisme untuk melakukan filter pre processing sebelum sebuah context (request dan response) diproses pada route atau didelegasikan ke sub bundle, dan/atau post processing setelahnya.

Middleware dapat mengakses context yang terdiri atas request dan response. Kita dapat melakukan filtering dan perubahan pada request object dan response sebelum dan sesudah pemrosesan dilakukan di route.

Middleware disusun dan dijalankan secara sekuensial dalam bentuk pemanggilan fungsi ke dalam diawali dengan bundle dispatch dan diakhiri dengan penanganan oleh route. 

Berikut ini adalah contoh konfigurasi yang memuat middleware-middleware yang digunakan dalam proses sebelum route dilakukan.

{% highlight python linenos %}
- middlewares:
    - middleware1
    - middleware2
    - middleware3
- route
    - route1
{% endhighlight %}

Untuk urutan middleware seperti tersebut di atas maka berikut di bawah ini adalah urutan jalannya middleware.

{% highlight python linenos %}
middleware1:
    internal preprocess middleware1
    call middleware2
    internal postprocess middleware1

middleware2:
    internal preprocess middleware2
    call middleware3
    internal postprocess middleware2

middleware3:
    internal preprocess middleware3
    call route1
    internal postprocess middleware3
{% endhighlight %}

### Mendefinisikan Middleware
<hr>

Aplikasi dapat menggunakan middleware yang sudah dibuat dalam bentuk pustaka oleh orang ketiga ataupun middleware bawaan dari Bono. Beberapa middleware bawaan Bono antara lain, [ContentNegotiator](ContentNegotiator), [BodyParser](BodyParser), dll. Middleware yang dibuat oleh pihak ketiga dapat ditambahkan melalui Composer.

Untuk mendefinisikan pemakaian middleware pada bundle yang kita inginkan dapat dilakukan dengan beberapa cara,

#### Mendefinisikan Middleware melalui konfigurasi Bundle

Apabila diinginkan untuk mendefinisikan middleware dengan memanfaatkan penggunaan Bono Dependency Injection, maka middleware dapat juga didefinisikan melalui berkas konfigurasi.

{% highlight php linenos %}
<?php
use Some\ThirdParty\Middleware;
use Another\Middleware as Middleware2;

return [
    'middlewares' => [
        // membutuhkan parameter
        [ Middleware:class, [
            'options' => [ ... ],
        ]],
        
        // tanpa parameter
        [ Middleware2:class, ],
        
        // function middleware
        function (Context $context, $next) {
            // do something here
            $next($context);
        },
    ],
];
{% endhighlight %}

#### Mendefinisikan Middleware Konkrit

Penggunaan middleware pada bundle tertentu dapat dilakukan dengan menambahkan instance konkret dari sebuah class yang memiliki `__invoke` method, sehingga instance tersebut dapat dipanggil sebagai fungsi.

{% highlight php linenos %}
<?php
use Bono\Bundle;
use Some\ThirdParty\Middleware;

$bundle = new Bundle();

// mendefinisikan konkret middleware
$middleware = new Middleware();
$bundle->addMiddleware($middleware);
{% endhighlight %}

#### Mendefinisikan Middleware dengan Metadata

Selain menambahkan instance dalam wujud konkret, dapat pula ditambahkan dalam wujud metadata yang disepakati oleh `Injector` dari mekanisme Dependency Injection yang digunakan oleh Bono. Dalam hal ini Bono menggunakan `reekoheek/util` versi `^1.1.0`.

{% highlight php linenos %}
<?php
use Bono\Bundle;
use Some\ThirdParty\Middleware;

$bundle = new Bundle();

// mendefinisikan meta middleware
$bundle->addMiddleware([ Middleware::class, [
    'arg1' => $arg1,
    'arg2' => $arg2,
]]);
{% endhighlight %}

#### Mendefinisikan Middleware berbentuk Function

Penggunaan dengan cara di atas memerlukan pustaka pihak ketiga atau mendefinisikan class yang bersangkutan pada aplikasi kita. Middleware dapat juga didefinisikan sebagai function apabila diinginkan untuk menggunakan middleware sederhana secara custom tanpa harus mendefinisikan class terlebih dahulu.

{% highlight php linenos %}
<?php
use Bono\Bundle;
use Some\ThirdParty\Middleware;

$bundle = new Bundle();

// mendefinisikan function middleware
$bundle->addMiddleware(function(Context $context, $next) {
    // do something here to preprocess
    $next($context);
    // do something here to postprocess
});
{% endhighlight %}

### Menambahkan Middleware secara Custom
<hr>

Apabila dari bawaan Bono maupun pustaka pihak ketiga tidak terdapat middleware yang ingin digunakan, maka kita dapat membuat middleware sendiri dalam bentuk class di dalam aplikasi kita maupun dipaketkan pada pustaka terpisah. Tujuannya adalah agar middleware tersebut dapat digunakan ulang pada kesempatan pembangunan aplikasi yang lain.

Cara membuat middleware secara custom adalah dengan cara membuat class yang memiliki function `__invoke`.

{% highlight php linenos %}
<?php

namespace MyLib;

class MyMiddleware 
{
    public function __construct($somearg, array $options = [])
    {
        // ...
    }

    public function __invoke(Context $context, $next) 
    {
        // pre processing
        
        $next($context);
        
        // post processing
    }
}
{% endhighlight %}

Berikut ini adalah penjelasan dari definisi class sebagai middleware di atas.

- Baris #3, mendefinisikan namespace untuk membedakan paketan dari middleware yang dibuat. Untuk sebuah middleware yang kita buat secara khusus internal pada sebuah aplikasi maka namespace yang digunakan adalah namespace aplikasi maupun sub namespacenya. Untuk middleware yang akan kita paketkan sebagai distribusi dari sebuah pustaka, maka penamaan tersebut adalah namespace dari pustaka maupun sub namespacenya.
- Baris #5, middleware dapat didefinisikan sebagai class umum (tanpa harus extends dari class khusus, berbeda dari Bono 1). Yang terpenting, class tersebut harus memiliki fungsi `__invoke`.
- Baris #7, middleware dapat mendefinisikan constructor dengan parameter. Jika middleware tersebut ditambahkan ke bundle dengan cara pendefinisian metadata atau konfigurasi, maka parameter-parameter tersebut akan diisi oleh [Dependency Injection Injector](../dependencyInjection).
- Baris #12, middleware harus mendefinisikan fungsi `__invoke`, yang akan dipanggil untuk setiap datangnya context request response.

### Middleware Bono
<hr>

Berikut ini adalah beberapa middleware bawaan Bono.

- [BodyParser](BodyParser)
- [ContentNegotiator](ContentNegotiator)
- [MethodOverride](MethodOverride)
- [Notification](Notification)
- [Profiler](Profiler)
- [StaticPage](StaticPage)
- [TemplateRenderer](TemplateRenderer)

### Middleware Pihak Ketiga
<hr>

Berikut ini adalah beberapa middleware pihak ketiga yang umum digunakan dalam pembangunan aplikasi berbasis Bono.

- [Norm](https://github.com/xinix-technology/bono-norm)

Jika anda telah membuat pustaka yang ingin dimasukkan dalam daftar di atas, hubungi maintainer Bono ;).
