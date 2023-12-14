# Cloudnet data portal documentation

Source for the documentation hosted at <https://docs.cloudnet.fmi.fi/>.

## Development

Serve documentation locally by running:

```sh
docker run --rm --volume="$PWD:/srv/jekyll:Z" --publish [::1]:4000:4000 jekyll/jekyll:4.2.0 jekyll serve
```
