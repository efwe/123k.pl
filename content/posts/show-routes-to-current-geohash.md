---
layout: post
title: "Show routes to current geohash"
date: 2016-01-2016-01-07T09:29:50+01:00
categories: [leaflet, mongodb, geohashing]
---
# The Plan
My geohashing alert2016-01-13 15:28-system at [123k.org](https://123k.org/geohashing) shows the current [geohash](http://wiki.xkcd.com/geohashing/) for my home [graticule](https://en.wikipedia.org/wiki/Geographic_coordinate_system). The plan is to the nearest route which passes by.

# Finding Something
As mentioned [earlier]({{< ref "build-a-coverage-map-with-mongodb.md" >}}) I have all points i ever visited in a collection.
This collection holds a [geospatial index](https://docs.mongodb.org/manual/applications/geospatial-indexes/). Therefore I can issue mongo queries with
the `$nearSphere` command. This command finds the first points from a given location.

{{< gist efwe c73ab0d887110156c2f9 "first_route.rb" >}}

So this just works. But what about other routes which may also be nice and just around the corner?
We can select many points around the hash point and check which routes they're on.

{{< gist efwe dfc73a236cfc94ef3313 "routes_to.rb" >}}

The trick here is that we have a implicit `distinct` by grouping the points according to their route-id. So we really find
every route within 5km just once.

So this also works, but it is not 100% predictable _how many_ routes we really find. If the point sits just around the corner,
this algorithm will find tons of routes which then are all queried and displayed. Also not useful. I'll have to invent a custom
`limit` here. Or I find a solution to limit the results of the aggregation run - this is just open now.

I think I'll stick with the first approach as it nicely stops finding things after the first point and as nobody really
uses my site anyways I provide the second approach via button click.

# Showing The Routes
Showing the routes is dead easy again. We just fetch some geo-json and add it to the map:

{{< gist efwe c78ad24bad8368412b4f "show_routes.coffee" >}}

As my geohashing page shows the whole graticule, but my bike-rides just cover some 20km radius I only display the
route when the point sits within 35km of my home. So we need a distance calculation. This is again easy as leaflet just provides
this.

{{< highlight coffee-script >}}
if L.latLng(@location.lat, @location.lon).distanceTo(L.latLng(lat, lon)) < 35000
  @firstRouteToPoint([lon, lat])
{{</ highlight >}}


As this feature is not visible every day I took a screenshot for reference. You can see, that the additional routes are not
100% useful, especially because they are not clickable yet. But this is another feature.

![displayed routes](/images/displayed_routes.png)

The first route is not so bad. It looks good but is also not clickable yet.

![first](/images/first_route.png)
