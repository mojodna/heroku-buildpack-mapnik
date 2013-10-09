# heroku-buildpack-mapnik

Heroku buildpack that installs Mapnik and its dependencies.

## Testing

```bash
cd /tmp
mkdir mapnik-app
cd mapnik-app
git init
heroku apps:create -p -b https://github.com/mojodna/heroku-buildpack-multi.git#build-env
echo https://bitbucket.org/mojodna/heroku-buildpack-mapnik-deps.git >> .buildpacks
git add .buildpacks
git commit -m "test"
```

## Building

```bash
curl -LO https://github.com/mapnik/mapnik/archive/2.3.x.tar.gz -o mapnik-2.3.x.tar.gz
tar zxf mapnik-2.3.x.tar.gz
cd mapnik-2.3.x
LD_LIBRARY_PATH=/app/vendor/icu/lib:/app/vendor/proj/lib:/app/vendor/boost/lib:/app/vendor/gdal/lib ./configure ICU_INCLUDES=/app/vendor/icu/include ICU_LIBS=/app/vendor/icu/lib PROJ_INCLUDES=/app/vendor/proj/include PROJ_LIBS=/app/vendor/proj/lib BOOST_INCLUDES=/app/vendor/boost/include BOOST_LIBS=/app/vendor/boost/lib GDAL_CONFIG=/app/vendor/gdal/bin/gdal-config PREFIX=/app/vendor/mapnik BINDINGS=none CPP_TESTS=no
LD_LIBRARY_PATH=/app/vendor/icu/lib:/app/vendor/proj/lib:/app/vendor/boost/lib:/app/vendor/gdal/lib make -j4
make install
cd /app/vendor/mapnik
tar zcf /tmp/mapnik-2.3.x-1.tar.gz .
```
