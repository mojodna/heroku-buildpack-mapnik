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
