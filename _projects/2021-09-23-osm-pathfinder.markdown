---
date:   2021-09-23 10:55:32 -0400
categories: projects
title: "Open Street Maps Pathfinder"
excerpt: "Recursive pathfinder program using real world street data"
header:
  path: /assets/images/valorant-aimbot-header.png
  overlay_image: /assets/images/valorant-aimbot-header.png
  teaser: assets/images/OSM-pathfinder-header-thin.png
  actions:
    - label: "View on github"
      url: "https://github.com/obtusecanadiangoose/Open-Street-Map-Pathfinder"
sidebar:
  - title: "Goals:"
  - text: "Create a path of at least a certain distance"
  - text: "Not get stuck (avoid dead-ends)"
  - text: "Fewest intersections as possible"
  
  - title: "Experience Gained:"
  - text: "Open Street Map data - Nodes, Paths, and Edges"
  - text: "Recursive Algorithms"
---
*Note, this program is best used in semi-rural areas, as dense clusters of nodes (ie, cities) can cause noticeable performance loss.*

The original concept for this project was to get mileage in while living in an urban environment, but based on the 
number of nodes in urban areas, it turned into more of an exercise in pathfinding. Data is taken from Open Street Maps (OSM).

## General Concept:
1. At every node, which in OSM represents intersections of "edges" (roads), find the 4 different edges that branch off of the node
    - This is accomplished by adding/subtracting 0.0001 to the latitude and longitude and then calling get_nearest_edge from osmnx
    - If this edge does not lead back to the origin, is not already in the current path, is not "blacklisted" (leads to a dead end) and
    is not already in the list of possible edges, add it.
2. Find the length of each of the possible edges, and choose the longest.
3. Check the total path length, if it is greater than or equal the desired distance, stop. If not, go to 1.
4. If step 1. finds no possible routes, this is dead end. The dead end node is added to a blacklist.
5. Backtrack to the previous node, if there is another edge that leads to a node that is not already in the path or blacklisted, carry on to that.
    - If no nodes are found at the previous node, backtrack until you find a node that works. 
6. When the desired path is finalized, the blacklisted nodes are removed from the path list and the final path is displayed.

If that was at all confusing, here's a demo:

<figure style="width: 769px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/osm-pathfinder-animation.gif" alt="">
</figure> 


*An example of a path with a minimum distance of 10k*