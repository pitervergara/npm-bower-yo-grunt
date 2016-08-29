# Container with node, npm, bower, grunt and yeoman packages.

Current build has the following versions:
- node v6.2.0
- npm v3.10.6
- bower v1.7.9
- yo v1.8.4
- grunt-cli v1.2.0
- grunt v0.4.5


User inside container is:
**uid=1000(user) gid=1000(user)**

There is an entrypoint script (_/entrypoint.sh_) which runs `npm install` and `bower install` every time you initiate the container with the default command, which is `grunt serve`.

## How to use it

### If you already have a node app

* Simply run the container mounting your files inside the it (at **/home/user/src**):
```bash
docker run --rm -it -v "$PWD:/home/user/src" -p 9000:9000 -p 35729:35729  pitervergara/npm-bower-yo-grunt
```
Your npm and bower requirements will be installed by the entrypoint script when you run the container.

### If you do not have an app yet
* From an empty directory, run the container mounting current folder inside it (at **/home/user/src**):
```bash
docker run --rm -it -v "$PWD:/home/user/src" pitervergara/npm-bower-yo-grunt /bin/sh
```

* Once inside the container, use yo to scafold a new app
```bash
npm install -g generator-karma
npm install -g generator-angular
yo angular  # answer the questions to create the app
exit
```

* Update you Gruntfile.js to listen on all addresses instead of only _localhost_ 

* Run your brand new app
```bash
docker run --rm -it -v "$PWD:/home/user/src" -p 9000:9000 -p 35729:35729  pitervergara/npm-bower-yo-grunt
```

### If you need to extend the container

* Create a `Dockerfile` inside your project folder containing:
```bash
FROM pitervergara/npm-bower-yo-grunt

# add your customizations like...
USER root
RUN apk add autoconf

USER user
```

* Build the container, running the following from within your project folder
```bash
docker build -t my-custom-container .
```

* Than, run your custom container
```bash
docker run --rm -it -v "$PWD:/home/user/src" my-custom-container
```