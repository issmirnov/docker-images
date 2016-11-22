# Node + Mongo + Go

This image build upon node-mongo to add go.
You might be wondering why I don't simply pull from the official go
image and tack on nodejs/mongo. The answer is that the mongodb alpine
image I found only works with the bleeding edge release of alpine. Hence,
for now I'm copy pasting the config from the tree recipes and hosting this.

All credit goes to https://github.com/docker-library/docs/tree/master/golang

This is a temporary image, here until all three components run on alpine stable.
