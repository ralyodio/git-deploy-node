git-deploy-node
===============

Deploy a node app using git for continuous deployment.


How to setup a git deploy to a production server.
====

### production server

	ssh example.com

# this is where you will deploy to
	mkdir p ~/deploy/example.com

# this is where the post-hook will export production code to (and served from by nginx)
	mkdir -p ~/www/example.com

# setup deploy branch
	cd ~/deploy/example.com
	git init --bare

# copy post-receive hook into repo
	cp ~/projects/git-deploy-node/post-receive ~/deploy/example.com/hooks/

Modify post-receive as needed. This is your build process.

	npm install
  npm update
	grunt deploy


### local dev machine (laptop)

# add remote named 'live' to deploy to from local working copy
	git remote add live ssh://example.com/home/user/deploy/example.com

# list remotes
	git remote

# deploy master branch to production
	git push live master
