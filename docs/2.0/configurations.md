---
layout: docsv2
title: Konfigurasi
---

Secara default Bono menggunakan Dependency Injection dalam kerjanya, konfigurasi secara konvensi akan berada pada direktori `config`. Berikut ini adalah struktur file dari aplikasi berbasis Bono pada umumnya.

{% highlight python linenos %}
- APP
    - config
        - config.php
        - config-development.php
        - config-production.php
    - src
        - ...
    - templates
        - ...
    - www
        - index.php
    - composer.json
{% endhighlight %}

`config/config.php` berisi konfigurasi default dari aplikasi. Sedangkan file-file `config/config-development.php` dan `config/config-production.php` adalah file konfigurasi spesifik terhadap environment.

Misalnya untuk environment production maka konfigurasi yang digunakan adalah `config/config.php` digabungkan dengan `config/config-production.php`.

### Instansiasi Metadata menjadi Konkret Instans
<hr>

Konfigurasi Bono menggunakan pustaka `reekoheek/util`. Class `Injector` sebagai Dependency Injection dan class `Options` untuk menggabungkan dari parameter maupun dari file konfigurasi spesifik untuk environment.

Berikut ini adalah contoh instansiasi dari metadata menggunakan Injector.

{% highlight php linenos %}
<?php

class Foo 
{
    public function __construct($bar, $baz)
    {
        // ...
    }
}

$metadata = [ Foo::class, [
    'baz' => 'This is baz',
    'bar' => 'This is bar',
]];

$app = Bono::getInstance();
// instansiasi menggunakan Bono App sebagai injector
$instance = $app->resolve($metadata); 
// sama dengan instansiasi umum
$instance = new Foo('This is bar', 'This is baz');
{% endhighlight %}

### Contoh Konfigurasi Aplikasi Web
<hr>

Berikut ini adalah contoh konfigurasi aplikasi secara umum

{% highlight php linenos %}
<?php

return [
    'middlewares' => [
        [ Bono\Middleware\MethodOverride::class, ],
        [ Bono\Middleware\TemplateRenderer::class, [
            'options' => [
                'renderer' => [ TRenderer::class, ],
            ],
        ]],
        [ Bono\Middleware\ContentNegotiator::class, ],
        [ Bono\Middleware\StaticPage::class, ],
        [ Bono\Middleware\BodyParser::class, ],
        [ Bono\Middleware\Notification::class, ],
    ],
    'bundles' => [
        [
            'uri' => '/user',
            'handler' => [ ROH\BonoNorm\Bundle\Norm::class, ],
        ],
    ],
    'routes' => [
        [
            'uri' => '/',
            'handler' => function ($context) {
                return [];
            }
        ],
    ]
];
{% endhighlight %}