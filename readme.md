# Shellcromancer.io Blog

Static blog site using Hugo to generate the content from markdown files & using Cloudflare Workers Sites to deploy it.

## All you need to know

Make sure to clone with submodules.
```
git clone --recurse-submodules git@github.com:shellcromancer/shellcromancer-io.git
```

Generating the Hugo Site.
```
hugo
```

Deploying w/ Cloudflare Wrangler.
```
wrangler publish
```
