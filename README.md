git-deploy-node
===============

Deploy a node app using git for continuous deployment.

How to setup a git deployment to a production server:

### production server

	ssh example.com

this is where you will deploy to

	mkdir p ~/deploy/example.com

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

### local dev machine (laptop)

add remote named 'prod' to deploy to from local working copy

	git remote add prod ssh://example.com/home/user/deploy/example.com

List remotes

	git remote -v

Deploy master branch to production

	git push prod master
