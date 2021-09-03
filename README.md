# FaaS Static WebApp Example

This is an example function running `of-watchdog` on `static` mode.

## Replicating

This example is based on `Dockerfile` template and it's very easy to replicate. Using `faas-cli`, create a new function; the example below is specifying `src` as the handler path, which replicates exactly the structure of this project, but you can modify that to suit your needs.

```bash
$ mkdir example
$ faas-cli new example --lang Dockerfile --handler src
No templates found in current directory.
Attempting to expand templates from https://github.com/openfaas/templates.git
Fetched 13 template(s) : [csharp dockerfile go java11 java11-vert-x node node12 node14 php7 python python3 python3-debian ruby] from https://github.com/openfaas/templates.git
Folder: src created.
  ___                   _____           ____
 / _ \ _ __   ___ _ __ |  ___|_ _  __ _/ ___|
| | | | '_ \ / _ \ '_ \| |_ / _` |/ _` \___ \
| |_| | |_) |  __/ | | |  _| (_| | (_| |___) |
 \___/| .__/ \___|_| |_|_|  \__,_|\__,_|____/
      |_|


Function created in folder: src
Stack file written: example.yml
```

The example contains a `function` folder inside the `src` folder. This is advisable, but if you want to follow your own structure, make sure you do the necessary adjustments in the `Dockerfile`.

The default `Dockerfile` is based on the classic template. Replace its contents with the one from this example. You'll then need to change the part that generates the static files. Remove the example instruction and replace with your build process.

```Dockerfile
# TODO: build project and generate static files
#RUN ....npm or whatever...
RUN mkdir public && cp index.html public/
#
```

The example also assumes that the static files are put in the `public` folder. If this is not the case, you'll need to change two more lines; this one

```Dockerfile
COPY --from=build /home/app/public public
```

And this one

```Dockerfile
ENV static_path="/home/app/public"
```

That should be all.
