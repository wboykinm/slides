# Raster is a disaster, vector is a spectre
The tale of one startup on a budget, wading through the tile wars

August 18th, 2017
---

# Remove later
Interactive mapping is a battlefield: vector tiles are the alluring wave of the future, but raster tiles are quick and reliable. What's a startup to do? What follows is the tale of Faraday Inc, a map-driven analytics company with a dedication to FOSS technologies, a willingness to build from the ground up, and a familiar-to-the-industry limit on engineering resources. We'll walk through comparisons of servers and clients, benchmark crazy geometry experiments, and rediscover some ancient carto-lore we should have paid attention to in the first place, before revealing where we arrived: at an optimal stack for mapping on the web in 2017.

I am:

[Bill Morris](https://twitter.com/vtcraghead), of [Faraday Inc.](https://www.faraday.io/)

---

What I do:

![data plumbing gif of some sort](https://www.dropbox.com/s/4zy5iur3y57c8q9/docbrown.gif?dl=1)

---

The Faraday Map Trajectory

1. Carto[DB]
2. Tilestache + Leaflet + D3
3. Tilesplash + Leaflet + D3
4. Tilesplash + Leaflet + Hoverboard
5. Tilesplash + MapboxGL
6. Rust Tiles + Leaflet