# Raster is a disaster, vector is a spectre

_The tale of one startup on a budget, wading through the tile wars_

FOSS4G Boston | August 18th, 2017

<notes>Interactive mapping is a battlefield: vector tiles are the alluring wave of the future, but raster tiles are quick and reliable. What's a startup to do? What follows is the tale of Faraday Inc, a map-driven analytics company with a dedication to FOSS technologies, a willingness to build from the ground up, and a familiar-to-the-industry limit on engineering resources. We'll walk through comparisons of servers and clients, benchmark crazy geometry experiments, and rediscover some ancient carto-lore we should have paid attention to in the first place, before revealing where we arrived: at an optimal stack for mapping on the web in 2017.</notes>

---

I am:

[Bill Morris](https://twitter.com/vtcraghead), Cartographer at [Faraday Inc.](https://www.faraday.io/)

---

What I do:

![data plumbing gif of some sort](https://www.dropbox.com/s/4zy5iur3y57c8q9/docbrown.gif?dl=1)

---

 - __What is Faraday?__
 - How does mapping fit in at Faraday?
 - How do tiles power the Faraday map?

---

Customer Intelligence Platform

---

![platform](https://www.dropbox.com/s/ry9ctlqdr076buz/Screenshot%202017-08-16%2023.22.21.png?dl=1)

---

This requires piles of data

---

![fig](https://www.dropbox.com/s/b766m26j8izfnmk/fig.gif?dl=1)

---

 - What is Faraday?
 - __How does mapping fit in at Faraday?__
 - How do tiles power the Faraday map?

---

Phase 1: Just let [Carto[DB]](https://faraday.carto.com/builder/f056ea4e-7758-11e5-b0b9-0ea31932ec1d/embed) do it for us

---

![cartodb.js_dots](https://www.dropbox.com/s/k4irnz2t5l0ibxq/12521606114_6f51b71226_o.png?dl=1)

---

![cartodb_ui_hexes](https://www.dropbox.com/s/wm9fve8wc2qjgxm/12481925823_0de4e3eb99_o.png?dl=1)

---

Owning the cartographic design

---

![prototype](https://cloud.githubusercontent.com/assets/735463/2822550/01df4cbe-cf14-11e3-815d-7b6f25a43572.png)

---

Which was inspired by . . .

---
![olivia](https://cloud.githubusercontent.com/assets/735463/2822443/c5ee7f00-cf12-11e3-8f14-dc3b12dde4ce.png)
<notes>Credit: "Olivia Forms a Band" by Ian Falconer</notes>


---

. . . the color scheme in one of my sons' favorite bedtime books.

---

Briefly jump ahead a few years . . .

![map](https://www.dropbox.com/s/mn163g1raefp5il/Screenshot%202017-08-16%2023.31.51.png?dl=1)

---

Back to the early days:

---

OH @faradayio: "We've heard vector tiles are awesome; let's try them out!"

---

 - What is Faraday?
 - How does mapping fit in at Faraday?
 - __How do tiles power the Faraday map?__

---

Phase 2: Use [Tilestache](http://tilestache.org/) (Thanks, [@michalmigurski](https://twitter.com/michalmigurski)), serve GeoJSON into a hacked-together client

---

![artifact_hexes](https://www.dropbox.com/s/w954kb9qhx25lgn/13126900385_8e7cd637d9_o.png?dl=1)

---

Phase 3: Build [Tilesplash](https://github.com/faradayio/tilesplash), serve TopoJSON into a [more-reliably-hacked-together client](http://bl.ocks.org/wboykinm/7393674) (Thanks, [@nelson](https://twitter.com/nelson))

---

![pitt_1](https://www.dropbox.com/s/9lyeisl3uci528n/Screenshot%202015-08-27%2009.35.24.png?dl=1)

---

![pitt_2](https://www.dropbox.com/s/c2cymxfa62tgxo9/Screenshot%202015-08-27%2009.39.14.png?dl=1)

---

Scale troubles:
![tile_load](https://www.dropbox.com/s/bej6deko227otw6/Screenshot%202015-10-02%2011.58.32.png?dl=1)

---

Phase 4: Update Tilesplash to serve [MVT](https://www.mapbox.com/vector-tiles/specification/)

---

Phase 5: Rewrite the client. Twice. Try out [the new Mapbox GL hotness](https://twitter.com/vtcraghead/status/887698981303832576).

---

. . . and prepare to introduce multivariate maps.

<notes>Revisit the design for multivariate, with implications for the tiling</notes>

---

![multivar_choropleth](https://www.dropbox.com/s/jv0cv9tkvs8pkcl/Screenshot%202016-11-22%2018.44.53.png?dl=1)

---

![circles](https://www.dropbox.com/s/9rkgkm4z84g58ke/nested_graduated_symbol.png?dl=1)

---

![3d_hexes](https://www.dropbox.com/s/4kj7fmdrhuj1lvn/Screenshot%202016-10-27%2012.07.56.png?dl=1)

---

![dots](https://www.dropbox.com/s/ikjk5dk3gr7u1hx/Screenshot%202016-11-22%2019.35.42.png?dl=1)

<notes>This last one is promising - it lets us sidestep the multivariate acarto-tricks and just show the raw data, but it's slow when served by tilesplash to MapboxGL. OH is only(!) 10M points, but we have way more than that nationally.</notes>

---

Phase 6: Start over. Consider old adages.

---

"What if we . . . drew dots on a bitmap really fast?"

---

Enter [Rust](https://www.rust-lang.org/en-US/), and "Grout".

<notes> . . . the work of Eric Kidd and Tristan Davies</notes>

---

Node begs PostGIS for some coords:

---

![PostGIS abides](https://www.dropbox.com/s/yaesomstca1eazt/Screenshot%202017-08-14%2016.07.19.png?dl=1)

---

Rust devours the output, renders pngs:

---

![drawcircles](https://www.dropbox.com/s/4agbmdlx1rrq306/Screenshot%202017-08-14%2016.11.33.png?dl=1)

---

Current status: __Speed and Stability__

- PostGIS on Citus (which is on AWS)
- NodeJS + Rust for server-side tiling (Containerized on AWS ECS)
- Mapbox.js up front (Also containerized on AWS ECS)

---

![final](https://www.dropbox.com/s/77bcj3cl70lmt6t/Screenshot%202017-08-16%2015.08.28.png?dl=1)

---

Q: Why might you go raster these days?

---

A: Speed + reliability in very narrow, million-point use cases

---

Q: Why might you just ride the vector wave?

---

A: Lord. A million reasons, but [here's one from just this week](https://developmentseed.org/blog/2017/08/09/mapbox-query-data/)

---

Thanks! Hit me up with any other questions:
[bill@faraday.io](bill@faraday.io)
[@vtcraghead](https://twitter.com/vtcraghead) on Twitter

