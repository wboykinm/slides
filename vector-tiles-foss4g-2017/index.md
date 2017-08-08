# Raster is a disaster, vector is a spectre

_The tale of one startup on a budget, wading through the tile wars_

August 18th, 2017
---

# Remove later
Interactive mapping is a battlefield: vector tiles are the alluring wave of the future, but raster tiles are quick and reliable. What's a startup to do? What follows is the tale of Faraday Inc, a map-driven analytics company with a dedication to FOSS technologies, a willingness to build from the ground up, and a familiar-to-the-industry limit on engineering resources. We'll walk through comparisons of servers and clients, benchmark crazy geometry experiments, and rediscover some ancient carto-lore we should have paid attention to in the first place, before revealing where we arrived: at an optimal stack for mapping on the web in 2017.

Incorporate: 
- https://github.com/faradayio/grout/blob/master/native/src/lib.rs
- https://github.com/faradayio/victor/blob/master/src/lib/dot.js

---

I am:

[Bill Morris](https://twitter.com/vtcraghead), Cartographer at [Faraday Inc.](https://www.faraday.io/)

---

What I do:

![data plumbing gif of some sort](https://www.dropbox.com/s/4zy5iur3y57c8q9/docbrown.gif?dl=1)

---

What is Faraday?

---

The Faraday map trajectory: 
__What the user sees__

---

![cartodb_ui_hexes](https://www.dropbox.com/s/wm9fve8wc2qjgxm/12481925823_0de4e3eb99_o.png?dl=1)

---

![cartodb.js_dots](https://www.dropbox.com/s/k4irnz2t5l0ibxq/12521606114_6f51b71226_o.png?dl=1)

---

"We've heard vector tiles are awesome; let's try them out!"

---

![artifact_hexes](https://www.dropbox.com/s/w954kb9qhx25lgn/13126900385_8e7cd637d9_o.png?dl=1)

---

![now_for_population_density](https://www.dropbox.com/s/vjyrmkn09v243e5/14947042810_63bbdcbc35_o.png?dl=1)

---

![pitt_1](https://www.dropbox.com/s/9lyeisl3uci528n/Screenshot%202015-08-27%2009.35.24.png?dl=1)

---

![pitt_2](https://www.dropbox.com/s/c2cymxfa62tgxo9/Screenshot%202015-08-27%2009.39.14.png?dl=1)

---

Scale troubles:
![tile_load](https://www.dropbox.com/s/bej6deko227otw6/Screenshot%202015-10-02%2011.58.32.png?dl=1)

---

![stabilizing_the_queries](https://www.dropbox.com/s/qpnutal1xzrhmbl/Screenshot%202016-07-13%2018.26.12.png?dl=1)

---

But what we really need is multivariate maps . . .

---

Time for some experiments

---

![circles](https://www.dropbox.com/s/9rkgkm4z84g58ke/nested_graduated_symbol.png?dl=1)

---

![3d_hexes](https://www.dropbox.com/s/4kj7fmdrhuj1lvn/Screenshot%202016-10-27%2012.07.56.png?dl=1)

---

![multivar_choropleth](https://www.dropbox.com/s/jv0cv9tkvs8pkcl/Screenshot%202016-11-22%2018.44.53.png?dl=1)

---

![dots](https://www.dropbox.com/s/ikjk5dk3gr7u1hx/Screenshot%202016-11-22%2019.35.42.png?dl=1)

---

![final](https://www.dropbox.com/s/weyrxkyg1t8y18i/Screenshot%202017-05-25%2010.12.16.png?dl=1)

---

The Faraday map trajectory:

__Tiles__

---

Phase 1: Just let [Carto[DB]](https://faraday.carto.com/builder/f056ea4e-7758-11e5-b0b9-0ea31932ec1d/embed) do it for us

---

Phase 2: Use [Tilestache](http://tilestache.org/) (Thanks, [@michalmigurski](https://twitter.com/michalmigurski)), serve GeoJSON into a hacked-together client

---

Phase 3: Build [Tilesplash](https://github.com/faradayio/tilesplash), serve TopoJSON into a [more-reliably-hacked-together client](http://bl.ocks.org/wboykinm/7393674) (Thanks, [@nelson](https://twitter.com/nelson))

---

Phase 4: Update Tilesplash to serve [MVT](https://www.mapbox.com/vector-tiles/specification/), try out [the new Mapbox GL hotness](https://twitter.com/vtcraghead/status/887698981303832576)

---

Phase 5: Benchmarking and recriminations

---

"What if we . . . drew dots on a bitmap really fast?"

---

Phase 6: Start over. Consider old adages. Try [Rust.](https://www.rust-lang.org/en-US/)

---

Current status: __Stability__

---

# prob. remove this too:

1. Carto[DB]
2. PostGIS + Tilestache + Leaflet + D3
3. PostGIS + Tilesplash + Leaflet + D3
4. PostGIS + Tilesplash + Leaflet + Hoverboard
5. PostGIS + Tilesplash + MapboxGL
6. PostGIS + Rust Tiles + Leaflet