# Fork of:
# Linaro's Automated Validation Architecture (LAVA) Docker Container
# i.e. (kernelci/lava-docker)

## What's different?

The output/ directory is committed to git.  This is not normal since those are
auto-generated files and configuration is meant to be done in boards.yaml.

But it was convenient to see what my actual configuration is now.  Also
I need to commit the dir because I added a Makefile there to help building and
running the container without docker-compose.  Mostly since at the moment I'm
only using the master part of the generated files, but also to hack directly on
the Dockerfile.

So as things are modified, it is not certain that boards.yaml and output/
content matches, and a lot of the stuff in the repo is not really needed by me
currently - but it just seemed convenient to fork the repo.

Now, please go look at upstream instead ;-)
https://github.com/kernelci/lava-docker

