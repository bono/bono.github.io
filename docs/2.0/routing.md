---
layout: docsv2
title: Routing
---

Routing adalah mekanisme utama dari Bono yang mengatur rute dari request-request yang datang untuk diproses pada fungsi yang tepat. Yang menjadi informasi bagi routing melakukan tugasnya adalah identitas uri yang disematkan pada konfigurasi rute.

{% highlight php linenos %}
<?php

return [
    // ...
    'routes' => [
        [ 'uri' => '/', function(Context $context) {

        }],
        [ 'uri' => '/foo', function(Context $context) {

        }],
        [ 'uri' => '/bar', function(Context $context) {

        }]
    ]
    // ...
]

{% endhighlight %}