Overview
========

The goal is to provide a benchmark suite, testing something representative
of real-world situations.

This script sets up nginx, siege, and PHP5/PHP7/HHVM over FastCGI, over a TCP
socket. Configuration is as close to identical as possible.

The script will run 300 warmup requests, then as many requests as possible in 1
minute. Statistics are only collected for the second set of data.

Results
=======

We don't have anything to share yet - we want to standardize and document
how the interpreters are built/installed first.

Usage
=====

As a regular user:

    hhvm perf.php --wordpress --php5=/path/to/bin/php-cgi # also works with php7
    hhvm perf.php --wordpress --hhvm=/path/to/hhvm

Running with --hhvm gives some additional server-side statistics. It is usual
for HHVM to report more requests than siege - some frameworks do call-back
requests to the current webserver.

Requirements
============

- nginx
- siege
- unzip
- A mysql server on 127.0.0.1:3306
- hhvm

I've been using the current versions available from yum on Centos 6.3. HHVM is required
as this is written in Hack.

The Targets
===========

Toys
----

Unrealistic microbenchmarks. We do not care about these results - they're only
here to give a simple, quick target to test that the script works correctly.

Wordpress
---------

- Data comes from installing the demo-data-creator plugin (included) on a
  fresh install of Wordpress, and clicking 'generate data' in the admin panel a
  bunch of times.
- URLs file is based on traffic to hhvm.com - request ratios are:

  100: even spread over long tail of posts
  50: WP front page. This number is an estimate - we get ~ 90 to /, ~ 1 to
      /blog/. Assuming most wordpress sites don't have our magic front page, so
      taking a value roughly in the middle.
  40: RSS feed
  5: Some popular post
  5: Some other popular post
  3: Some other not quite as popular post


The long tail was generated with:

    <?php
      for ($i = 0; $i <= 52; ++$i) {
      printf("http://localhost:__HTTP_PORT__/?p=%d\n", mt_rand(1,52));
    }

  Ordering of the URLs file is courtesy of the unix 'shuf' command.

SugarCRM
--------

Unrealistic microbenchmark: just the 'login' page. Added to confirm a
reported issue. The upstream installation script provides an option to create
demonstration data - this was used to create the database dump included here.

Laravel
-------

Unrealistic microbenchmark: just the 'You have arrived' page from an empty
installation.

Contributing
============

Please see [CONTRIBUTING.md](https://github.com/hhvm/oss-performance/blob/master/CONTRIBUTING.md) for details.
