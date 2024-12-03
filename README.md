# PHP Benchmark Script

A simple script that calculate execution time of common control flows
and function of the PHP. Helps you to compare performances of servers,
web hosting and also across PHP versions.

## Usage

From your SSH:

```sh
curl https://raw.githubusercontent.com/demartis/php-benchmark-script/master/bench.php | php
```

Or upload the file `bench.php` to your web document root and visit it.

#### Parameters

You can change the difficulty multiplier by passing a `multiplier` parameter:

```sh
php bench.php --multiplier=2
```

### Additional tests

You must have the `bench.php` file to your server.
You can download additional benchmarks (must be in the same directory as `bench.php`) using:

Available tests:
```sh
# rand: random number generation
wget https://raw.githubusercontent.com/demartis/php-benchmark-script/master/rand.bench.php
# io: file read/write/zip/unzip
wget https://raw.githubusercontent.com/demartis/php-benchmark-script/master/io.bench.php
# mysql: selects, updates, deletes, transactions, ....
# you need to pass the args --mysql_user=xxx --mysql_password=xxx --mysql_host=xxx (optional: --mysql_port=xxx --mysql_database=xxx)
wget https://raw.githubusercontent.com/demartis/php-benchmark-script/master/mysql.bench.php
```

Then you can run the benchmark using:

```sh
php bench.php
```

If the file is in the same directory as `bench.php`, it will be automatically loaded.

## Custom tests

You can create your own tests by creating a file in the same directory as `bench.php` and the file must be
named `*.bench.php`.

The file must return a closure, or an array of closures. Each closure should take a parameter `$multiplier` which is
how hard the test should be. The higher the `$multiplier`, the longer the test will take, by default it is `1`.
You should choose a reasonable number of iterations for your test (e.g. 1000) and multiply it by the `$multiplier`.

Example:

```php
// mytest.bench.php

return function ($multiplier = 1) {
    $iterations = 1000 * $multiplier;
    for ($i = 0; $i < $iterations; ++$i) {
        // do something
    }
};
```

Or with multiple tests:

```php
// mytest.bench.php

return [
    'my_test' => function ($multiplier = 1) {
        $iterations = 1000 * $multiplier;
        for ($i = 0; $i < $iterations; ++$i) {
            // do something
        }
    },
    'another_test' => function ($multiplier = 1) {
        $iterations = 1000 * $multiplier;
        for ($i = 0; $i < $iterations; ++$i) {
            // do something else
        }
    },
];
```

# Example Output

```
-------------------------------------------------------
|       PHP BENCHMARK SCRIPT v.2.1 by @DeMartis       |
-------------------------------------------------------
PHP............................................. 8.2.13
Platform........................................ Darwin
Arch............................................ x86_64
Server................................ MacRXintel.local
Max memory usage.................................. 128M
OPCache status................................. enabled
OPCache JIT....................... disabled/unavailable
PCRE JIT...................................... disabled
XDebug extension.............................. disabled
Difficulty multiplier............................... 1x
Started at..................... 03/12/2024 13:29:35.711
-------------------------------------------------------
math.......................................... 0.9236 s
loops......................................... 0.9303 s
ifelse........................................ 0.7111 s
switch........................................ 0.9627 s
string........................................ 1.1929 s
array......................................... 2.6089 s
regex......................................... 2.4980 s
is_{type}..................................... 0.4614 s
hash.......................................... 0.8476 s
json.......................................... 1.2549 s
-----------------Additional Benchmarks-----------------
io::file_read................................. 0.0469 s
io::file_write................................ 0.1634 s
io::file_zip.................................. 0.5763 s
io::file_unzip................................ 0.3416 s
mysql::ping................................... 0.0005 s
mysql::select_version......................... 0.0358 s
mysql::select_all............................. 0.6199 s
mysql::select_cursor.......................... 1.1740 s
mysql::seq_insert............................. 0.2086 s
mysql::bulk_insert............................ 0.3758 s
mysql::update................................. 0.2084 s
mysql::update_with_index...................... 0.2882 s
mysql::transaction_insert..................... 0.2419 s
mysql::aes_encrypt............................ 0.0388 s
mysql::aes_decrypt............................ 0.0373 s
mysql::indexes................................ 0.1708 s
mysql::delete................................. 0.1972 s
rand::rand.................................... 0.1551 s
rand::mt_rand................................. 0.1559 s
rand::random_int.............................. 0.2765 s
rand::random_bytes............................ 0.5079 s
rand::openssl_random_pseudo_bytes............. 2.1786 s
-------------------------Extra-------------------------
mysql::select_version::q/s....................... 28289
mysql::select_all::q/s............................ 1613
mysql::seq_insert::q/s............................ 4794
mysql::update::q/s................................ 4799
mysql::update_with_index::q/s..................... 4887
mysql::transaction_insert::t/s.................... 4135
mysql::aes_encrypt::q/s.......................... 27491
mysql::aes_decrypt::q/s.......................... 26896
mysql::delete::q/s................................ 5071
-------------------------------------------------------
Total time................................... 20.3907 s
Peak memory usage............................... 10 MiB

```

## Authors

- [@SergiX44](https://www.github.com/SergiX44)
- [@demartis](https://github.com/demartis)

## License

[MIT](https://choosealicense.com/licenses/mit/)

