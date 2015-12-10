Docker PHP Benchmark
====================

Docker resources to facilitate PHP benchmarks.

Includes docker files for:

 * neam/php-benchmark:ubuntu-15.04-fpm-5.6.4-hhvm-3.10.1-phalcon-2.0.9
 * neam/php-benchmark:ubuntu-15.04-fpm-7.0.0-hhvm-3.10.1-phalcon-2.0.9

Use to set up the following benchmarking environments:

* Ubuntu 15.04 64bit (Docker)
  * PHP-FPM 5.6.4
    * Zend OPcache 7.0.4-dev
    * PhalconPHP 2.0.9
  * PHP-FPM 7.0.0
    * Zend OPcache 7.0.6-dev
    * PhalconPHP 2.0.9
  * HHVM 3.10.1

By sharing underlying software stacks, the benchmark results vary only according to the host machine's hardware specs and differing code implementations.

License
-------

MIT