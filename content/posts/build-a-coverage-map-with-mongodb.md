---
title: "Build a Coverage Map With Mongodb"
date: 2016-01-02T19:38:06+01:00
---
# Introduction
When I started with my personal bike-track homepage at [123k.org](https://123k.org) a few years ago I decided to put all my tracks
in a MongoDB. The model I use is very simple. I have a class `Route` which holds meta-data about the track
and a class `TrackPoint` which represents a point and has a foreign key to the respective track (of course).
After some years I have 612827 track-points. So 600k entries is not 'big-data' exactly,
but it is too much to be displayed on a leaflet map at once. What I really wanted is a 'Coverage View', which just
shows me where I've been with the bike.

# The Algorithm

As I start all of my rides at my home and eventually return there I have a center where to start the coverage.
From there I started the following algorithm.

{{< gist efwe 65e28c72c9948da0e9f8 "coverage.rb" >}}


It does 100m steps for 50km and selects all points in the area between min/max distance.

{{< gist efwe 675f2d2d56470933aaa5 "hull_for.rb" >}}

The trick is, that I only consider the points of the [convex hull](https://en.wikipedia.org/wiki/Convex_hull).
I chose the Jarvis March algorithm as completely described [here](http://tomswitzer.net/2009/12/jarvis-march/).

This is very efficient, most of the points are not considered (A random output of my job is
`calculated hull polygon with 22 points from a total of 2217 points`). Finally I only have 12033 points left.
The resulting points are stored as one document in MongoDB.


{{< highlight ruby >}}
client = TrackPoint.mongo_client
client[:coverage].drop()
client[:coverage].insert_one({:points => points})
{{</ highlight >}}


You can see the result of this [here](https://123k.org/coverage).

