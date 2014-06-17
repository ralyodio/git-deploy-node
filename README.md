git-deploy-node
===============

Deploy a node app using git for continuous deployment.

## Install node.js

Follow the instructions from Joyent:
https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager


## Introduction

We will setup a remote repository just for deployments on the production server.

When you are finished, you will be able to do continuous deployment using the following command (from your local machine):

    git push prod master

### Setup production server

	ssh example.com

This is where you will deploy to

	mkdir -p ~/deploy/example.com

This is where the post-receive hook will export production code to (and served from by nginx)

	mkdir -p ~/www/example.com

Setup deploy branch

	cd ~/deploy/example.com
	git init --bare

Copy post-receive hook into repo

	cp ~/projects/git-deploy-node/post-receive ~/deploy/example.com/hooks/

Modify post-receive as needed. This is your build process.

	npm install
	NODE_ENV=prod grunt deploy

### Setup local dev machine (laptop)

Add a remote named 'prod' to deploy to from local working copy (this is where you will deploy from).

	git remote add prod ssh://example.com/home/user/deploy/example.com

List remotes

	git remote -v

Deploy master branch to production

	git push prod master

### Tips

You can repeat this process for 'stage' and 'test' environments as well. I usually use a separate sub-domain like 'test.example.com' for a new environment.

You can add a remote called 'test' instead of 'prod':

	git remote add test ssh://example.com/home/user/deploy/test.example.com

...and then deploy with:

    git push test master

Its a good idea to tag your release with the following command (although I always deploy master to environments, as the post-receive hook only supports master branch).

    git tag v0.0.1

### Installing node

For local installations where you do not have root:

    mkdir -p $HOME/opt/node

Download latest node source code and unpack, then build:

    ./configure --prefix=$HOME/opt/node
    make
    make install
