Heroku buildpack: lame
=======================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for using lame in your project.
It doesn't do anything else, so to actually compile your app you should use [heroku-buildpack-multi](https://github.com/ddollar/heroku-buildpack-multi) to combine it with a real buildpack.

Lineage
-------

Shamelessly forked from the [ffmpeg heroku buildpack](https://github.com/shunjikonishi/heroku-buildpack-ffmpeg) by [Shunji Konishi](https://github.com/shunjikonishi).

Usage
-----
To use this buildpack, you should prepare a .buildpacks file that contains this buildpack url and your real buildpack url:

    $ ls
    .buildpacks
    ...

    $ cat .buildpacks
    https://github.com/circonflex/heroku-buildpack-lame

    $ heroku create --buildpack https://github.com/ddollar/heroku-buildpack-multi

    $ git push heroku master
    ...

You can verify that lame is installed by calling:

    $ heroku run "lame"

Compiling Lame
--------------

Follow this script for recompiling 'lame' on Heroku:

    $ heroku create
    $ heroku run bash -a APP_NAME
    $ wget https://sourceforge.net/projects/lame/files/lame/3.100/lame-3.100.tar.gz
    $ tar xvzf lame-3.100.tar.gz
    $ cd lame-3.100
    $ ./configure
    $ make
    $ DESTDIR=/app make install
    $ cd ~
    $ mv /app/usr/local /app/usr/lame
    $ tar czfv lame.tar.gz /app/usr # the tar file must start with the directory 'lame'
    $ curl -F "file=@lame.tar.gz" https://file.io
    $ wget FILE_IO_URL
    $ mv FILE_NAME lame.tar.gz

Manual Verification
-------------------

Deploy a simple application to Heroku:

    $ git clone https://github.com/heroku/node-js-getting-started.git
    $ cd node-js-getting-started
    $ heroku create --buildpack https://github.com/circonflex/heroku-buildpack-lame
    $ git push heroku
    $ heroku run lame

For repeated deployments:

    $ git commit --allow-empty -m "Deploy"
    $ git push heroku

References
----------

* https://sendgrid.com/blog/create-first-heroku-buildpack/
* https://devcenter.heroku.com/articles/stack-packages