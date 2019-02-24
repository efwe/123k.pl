---
title: "Streaming Geo-Locations"
date: 2019-02-24T10:43:07+01:00
draft: false
---
# Streaming Server Experiments
My first tests with streaming geo-locations from mongodb to a webclient started. The results are pretty promising and I even
have two calls in production already.
 
## Mosty - Spring-Boot Webflux
My first try will be a Spring-Boot application named [Mosty](https://github.com/efwe/mosty)
(which is of course a small [village](https://goo.gl/maps/38TGkXUfZNM2) nearby and home of the infamous sandpit).
The result for now is basically a copy and paste from tutorials and stack-overflow - but it just work and the first
stream of points is already consumed by my new static homepage.

{{< highlight kotlin >}}
    @CrossOrigin
    @GetMapping(value = ["/track/{id}/point-stream"], produces = [MediaType.TEXT_EVENT_STREAM_VALUE])
    @ResponseBody
    fun trackPointStream(@PathVariable("id") trackId: String): Flux<ServerSentEvent<TrackPoint>> {
        return trackPointRepository.findByTrackId(ObjectId(trackId))
                .map { point: TrackPoint ->
                    ServerSentEvent
                            .builder<TrackPoint>()
                            .data(point)
                            .event("point")
                            .build()
                }

    }
{{</ highlight >}}




I was able to stream geo-locations and I even prepared a 'legacy' version of the same call, which collects all the data
on server side before sending it out. This call is even in production while switching tracks on the frontpage of 123k.org.
 
And I think I have to follow that path because of the Kotlin language, which I may should learn for future use.

## Choiny - Handrolled Go 
The second one I started is a Go version of the same service name [Choiny](https://github.com/efwe/choiny) (which is
of course the even smaller [village](https://goo.gl/maps/pR78PZjGTcL2) and a popular path to the east for me). 
This one can not do too much right now - only one mongo query and playback. Nothing from this is in production here. 
The plan is to provide the same API as Mosty does and compare the programming model, performance (Not really - bot streaming servers
 will be blazing fast for me) and everything in between. But this is a very ambitious goal - so for now I want to implement one or
 two requests to see how things are going. Especially because this language is also completely new - and I leave the sweet spot
 of the JDK eco-system the first time in 15 years :)


# Relocate everything to Hetzner Cloud
Because of the fact, that I want to put a JVM service into production for the first time in years I decided to split my setup up.
Instead of one freeBSD box which does everything, I know have two servers. 

## tontu.123k.org
[Tontu](http://tontu.123k.org) is the freeBSD frontend server which hosts the static website for [123k.work](https://123k.work) and
the rails installation for [123k.org](https://123k.org). 

## hilda.123k.org a.k.a map.123k.org
[Hilda](http://hilda.123k.org) is the Ubuntu Linux backend server which hosts the mongodb and the JVM service. It is referenced
as [https://map.123k.org](https://map.123k.org) from the frontend. The mongodb is still unsecured but listens on a public IP know - 
medium good idea as we learned in the past. For now I limit access via a firewall rule, but this is not optimal. 


# Outlook Stream Consumers - SSE vs. Websockets
All of the tutorials for Spring-Boot-Webflux somehow ended with a pointer to Websockets.
So as I have already experience with Websockets and I do not plan to use the main features of (full-duplex messaging, long-time-listening where events come late
after hours) I was not convinced. I just want to answer one request, with a little bit more result-data than usual so that it would be a good
idea to *not* buffer that stuff on the server but start streaming immediately. 

Because of this I stepped back to SSE - Server Send Events. From a Server point of view, they where really easy to generate.
But on the client the programming model feels a little bit strange. But for now it is a start. 

{{< highlight coffee-script >}}
switchTrack: (id) ->
  @trackLayer.clearLayers()
  polyline = L.polyline([], @lineOptions);
  @trackLayer.addLayer(polyline)

  pointEventHandler = (e) =>
    data = JSON.parse(e.data)
    polyline.addLatLng(L.latLng(data.location))

  eventSource = new EventSource(@mapApiEndpoint + '/track/' + id + '/point-stream')

  eventSource.onerror = () =>
    eventSource.close()
    @map.fitBounds(polyline.getBounds())

  eventSource.addEventListener('point', pointEventHandler, false)

{{</ highlight >}}


You can find the first test-results on my new static site [123k.work](https://123k.work).







