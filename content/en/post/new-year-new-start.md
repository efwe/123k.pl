---
title: "New Year New Start"
date: 2019-02-09T19:42:54+01:00
---
# Back in Business
Hello there, I'm [Back in Business](https://www.youtube.com/watch?v=ldeerlh8_DA) (jak EPMD!) - 
only after three years of effective inactivity I decided to re-active this blog again.
As I really did not prepare any content [^1] , I can only start with some small notes.

## Disaster and Recovery 
So in September 2018 I found out, that my server where the [123k](https://123k.org) project runs did not respond any more. 
I went to the fabulous [Hetzner](https://hetzner.de) site and found out, that it just does not boot
any more because of ZFS file system corruption. 
This was not recoverable - I saved a snapshot of the whole thing. So if
some ZFS developer wants to know what happened to this special file system - you're welcome.
I myself did not investigate too much and just setup a new server (so I do *not* know if I was hacked or the underlying hypervisor had a hickup).

But unfortunately I really lost data.
The well curated descriptions of my bike rides for the whole 2018 season were lost. 
I was able to recover the tracks itself from the raw
GPX data, but I ended up with [several](https://123k.org/geo/routes/5bd6024c97882f6d4b16225d) [tours](https://123k.org/geo/routes/5bd6052e97882f6d4b16604e) [with](https://123k.org/geo/routes/5bd6057297882f6d4b166cea) [placeholder](https://123k.org/geo/routes/5bd604d697882f6d4b1652ab) names and descriptions on the site. So just in case you wondered.

To bring you those news I wanted to write a blog post and noticed that my whole setup with
github-pages and [Octopress](http://octopress.org) and everything encountered a severe version of bit-rot.

## Welcome Hugo
So as the [Octopress](http://octopress.org) project coincidentally also started a longer break (no relevant activity for the last three years)
I gave up...  and just 5 months later I found time to restart the whole blogging bullshit with the now popular [Hugo](https://hugo.io).

The content itself aged pretty good of course and the new tooling concept incorporates only one binary installed
via snap - so let's see how the next generator will be named :)

## Wipu Quo Vadis?
So apart from the way-point feature I did not add anything new to the site. 
But I continued to really use and enjoy it. Especially because it really works.
Here are the top-features I use:

* I find (new) places I want to visit because of my coverage map. 
* I plan and execute geohashing campaigns (ok, to be fair - last year only one really)
* I enjoy clicking through descriptions of old rides (embarrassing I know)

So I think I will continue to support my site also in 2019. But to keep
things interesting (technically) I want to try one thing which grew upon me.
It really nags me every time I click on a route. I always ask myself, if it would be possible
to stream the track-points to show them incrementally on the map. This would provide a better
'feeling' for larger tracks for the existing use-cases and maybe could be a foundation to 
build more experimental new use-cases on top of it 
Something like running calculations which produce more points over a longer time because they are complicated
 but could not be generated a priori like the coverage. 

[^1]: I actually decided to cheat and produce an artificially dated article about my way-point feature 
