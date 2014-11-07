# heroku-buildpack-mapnik

**NOTE**: With the GA of Heroku's `cedar-14` stack and `node-mapnik`'s bundling
of binaries, this is no longer necessary (or useful in most circumstances, as it
does not include Python bindings).

I am a Heroku buildpack that installs [Mapnik](http://mapnik.org) and its
dependencies ([Boost](http://boost.org/), [GDAL](http://gdal.org/),
ICU(http://icu-project.org/), [proj](https://trac.osgeo.org/proj/), and
[Protocol Buffers](https://code.google.com/p/protobuf/)).

When used with
[heroku-buildpack-multi](https://github.com/ddollar/heroku-buildpack-multi),
I enable subsequent buildpacks / steps to link to these libraries.  (You'll
need to use the `build-env` branch of [@mojodna's
fork](https://github.com/mojodna/heroku-buildpack-multi/tree/build-env) for the
build environment (`CPATH`, `LIBRARY_PATH`, etc.) to be set correctly.)

*Note:* Python bindings are *not* currently included.

## Using

There are multiple variants present as branches. To use a specific variant, add
`#branch` to the Mapnik buildpack URL in `.buildpacks`.

* [master](https://github.com/mojodna/heroku-buildpack-mapnik/tree/master) -
  Mapnik 2.2
* [jemalloc](https://github.com/mojodna/heroku-buildpack-mapnik/tree/jemalloc) -
  Mapnik 2.2 w/ [jemalloc](http:/www.canonware.com/jemalloc/)
* [2.3.x](https://github.com/mojodna/heroku-buildpack-mapnik/tree/2.3.x) -
  Mapnik 2.3.x (see
  [`bin/compile`](https://github.com/mojodna/heroku-buildpack-mapnik/tree/2.3.x/bin/compile)
  for the revision provided).
* [2.3.x+jemalloc](https://github.com/mojodna/heroku-buildpack-mapnik/tree/2.3.x+jemalloc) -
  Mapnik 2.3.x with jemalloc
* [2.3.x-debug+jemalloc](https://github.com/mojodna/heroku-buildpack-mapnik/tree/2.3.x-debug+jemalloc) -
  Debug build of Mapnik 2.3.x with jemalloc

When creating a new Heroku app:

```bash
heroku apps:create -b https://github.com/mojodna/heroku-buildpack-multi.git#build-env

cat << EOF > .buildpacks
https://github.com/mojodna/heroku-buildpack-mapnik.git
https://github.com/heroku/heroku-buildpack-nodejs.git
EOF

git push heroku master
```

When modifying an existing Heroku app:

```bash
heroku config:set BUILDPACK_URL=https://github.com/mojodna/heroku-buildpack-multi.git#build-env

cat << EOF > .buildpacks
https://github.com/mojodna/heroku-buildpack-mapnik.git
https://github.com/heroku/heroku-buildpack-nodejs.git
EOF

git push heroku master
```

## Building

GDAL and proj were built according to [the steps in the GDAL buildpack](https://github.com/mojodna/heroku-buildpack-gdal#building).

Everything else was built in an Ubuntu 10.04 `chroot`. (See
[heroku/stack-images](https://github.com/heroku/stack-images) for package
listings and post-installation.)

`chroot` preparation:

```bash
mkdir app tmp
sudo /vagrant/bin/install-stack cedar64-2.0.0.img.gz
sudo mount -o bind /dev /mnt/stacks/cedar64-2.0.0/dev/
sudo mount -o bind /home/vagrant/tmp /mnt/stacks/cedar64-2.0.0/tmp/
sudo mount -o bind /home/vagrant/app /mnt/stacks/cedar64-2.0.0/app/
```

[Steps to be updated next time Mapnik or its dependencies are upgraded.]
