---
title: Where are all the Hungarians?
subtitle: Visualising the exodus of our workforce
header-url: https://images.unsplash.com/photo-1488782552271-d48484d86326?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1900&fit=crop&s=a19a23865110a05941c5056b9ad5b1a4
photo-author: Bethany Stephens
photo-author-profile: bethanyiam
date: 2017-09-07
category: data-viz
---

It's been a while since I played with some data visualisation in D3.js, but when I saw the chart below I decided to get back to it. Even though (or especially because) the topic is not a funny one.

## The source

The huge number of talented people leaving Hungary to work abroad is a broad and serious issue, and Portfolio.hu did an excellent job in debunking the clearly false (falsified?) official data from the government. The methodology and the numbers are not perfect but still the picture emerges from the numbers.

Here is the chart about their findings [from the article](http://www.portfolio.hu/gazdasag/ide_vezetett_a_tomeges_kivandorlas_tobb_magyar_lepett_le_mint_gondoltuk.1.260665.html).

![](/img/posts/migration.png)

## Giving it a boost

I thought that this issue might deserve some visually more appealing format than this dull bar chart. Don't get me wrong, I'm guilty enough in producing thousands of boring charts throughout my career, and in many cases an efficient, simple approach might be enough. 

But I had some time to spend on this, and here's what I managed to put together using the same numbers:

<div>
    <link href="https://fonts.googleapis.com/css?family=Encode+Sans+Condensed:500,600" rel="stylesheet">
    <style>
      #map {
        border: 1px solid #ddd;
        font-family: 'Encode Sans Condensed', sans-serif;
      }

      .loader {
        font-size: 12px;
        fill: #666;
        alignment-baseline: middle;
        text-anchor: middle;
      }

      .land {
        fill: #fff;
        stroke-width: 1;
        stroke: #eeeeee;
      }

      .member {
        fill: #eee;
        stroke: #aaa;
        stroke-width: 1;
      }

      .member:hover {
        fill: #EE9448;
        fill-opacity: 0.2;
      }

      .sea {
        fill: #f5f5ff;
      }

      .marker {
        pointer-events: none;
      }

      .marker circle {
        fill: #EE9448;
        fill-opacity: 0.8;
        stroke: #EE9448;
        stroke-width: 1;
      }

      .marker .active {
        fill-opacity: 1;
      }

      .legend {
        fill: transparent;
        stroke: #cccccc;
        stroke-width: 0;
      }

      .legend-label {
        font-size: 11px;
        fill: #666;
      }

      .data-label {
        font-size: 16px;
        fill: #444;
        line-height: 3em;
      }

      .data-label .highlight {
        font-weight: bold;
        font-size: 23px;
        fill: #EE9448;
      }

      .title {
        font-size: 30px;
        font-weight: bold;
        fill: #444;
      }

      .subtitle {
        font-size: 13px;
        font-weight: normal;
      }

      .reference {
        font-size: 11px;
        font-weight: normal;
        fill: #666;
      }
    </style>
    <svg width="700" height="600" id="map"></svg>
    <script src="https://d3js.org/d3.v4.js"></script>
    <script src="https://unpkg.com/d3-sankey@0"></script>
    <script src="https://unpkg.com/topojson-client@3"></script>
    <script>
      ;(function(window, document) {
      const EU_COUNTRY_CODES = ['040', '056', '100', '191', '196', '203', '208', 
      '233', '246', '250', '276', '300', '348', '372', '380', '428', '440', '442', 
      '470', '528', '616', '620', '642', '703', '705', '724', '752', '826'];

      // Base SVG element
      const svg = d3.select('#map');
      const width = +svg.attr('width');
      const height = +svg.attr('height');

      // Chart functions
      const format = d3.format(".2s")
      const projection = d3.geoMercator()
        .center([20, 47])
        .scale(680)
        .translate([width / 1.4 , height / 1.6]);

      const path = d3.geoPath(projection);
      const radiusScale = d3.scalePow()
      .range([5, 50])
      .exponent(0.5)
      .clamp(true);

      const ease = function(t) {
        return d3.easeElastic(t, 1.25, 0.35);
      };

      const map = svg.append('g').attr('class', 'map');

      // Add sea
      map.append('rect')
        .attr('class', 'sea')
        .attr('width', width)
        .attr('height', height);

      // Loader
      const loader = svg.append('g').attr('id', 'loader')
        .attr('transform', 'translate(' + width / 2 + ',' + height / 2 + ')')
      .append('text')
        .attr('class', 'loader')
        .text('Loading...');

      // Load map data
      d3.json("https://unpkg.com/world-atlas@1/world/110m.json", (error, world) => {
        if (error) throw error;

        let country_data = topojson.feature(world, world.objects.countries);

        // Load local data
        d3.json('/sample_data/data3.json', (error, data) => {
          if (error) throw error;
          loader.remove();
        
          // Add name and value to country data, sort by value
          const features = country_data.features.map(item => {
            found = data.find(country => country.id == item.id)
            if (found) {
              item.value = found.value;
              item.name = found.name;
            } else {
              item.value = 0;
              item.name = null;
            }
            return item
          }).sort((a, b) => a.value - b.value);

          // Set scale to the extent of data
          radiusScale.domain([
            1000, 
            data.map(i => i.value).reduce((a, b) => Math.max(a, b))
          ]);

          // Add background land shape
          map.append('path')
            .datum(country_data)
            .attr('class', 'land')
            .attr('d', path)
            .style('opacity', 0)
            .transition()
            .style('opacity', 1);

          // Add EU member countries on top
          map.selectAll('.country')
          .data(features)
          .enter()
          .filter(d => EU_COUNTRY_CODES.includes(d.id))
          .append('g')
            .classed('country', true)
          .append('path')
            .classed('member', true)
            .attr('d', path)
            .attr('id', d => d.id)
            .style('opacity', 0)
            .transition()
            .style('opacity', 1);

          // Add marker circles to countries
          map.selectAll('.marker')
          .data(features.filter(feature => feature.value > 0)
            .sort((a, b) => a.value - b.value ))
          .enter()
          .append('g')
            .attr('class', 'marker')
            .attr('id', d => 'marker_' + d.id)
            .attr('transform', d => {
              let x, y;
              if (d.id == '250') {
                // Manually adjust centroid of France
                x = path.centroid(d)[0] + 40;
                y = path.centroid(d)[1] - 35;  
              } else if (d.id == '191') {
                // Manually adjust centroid of Croatia
                x = path.centroid(d)[0] + 3;
                y = path.centroid(d)[1] - 10; 
              } else {
                // Make sure the marker is on the visible part of the map 
                x = Math.max(20, Math.min(width - 10, path.centroid(d)[0]));
                y = Math.max(20, Math.min(height - 10, path.centroid(d)[1]));
              }
              return 'translate(' + x + ',' + y + ')';
            })
          .append('circle')
            .attr('r', d => radiusScale(d.value))
            .style('opacity', 0)
            .transition()
            .delay(100)
            .style('opacity', 1)

          // Add listeners for mouse events
          map.selectAll('.member')
          .on('mouseenter', function(d) {
            const node = d3.select('#marker_' + d.id).raise();
            if (node.nodes().length > 0) {
              node.selectAll('circle').classed('active', true);

              let sideLabel = map.append('g')
                .attr('class', 'data-insert')
                .attr('transform', d => 'translate(20, 360)')
              .append('text')
                .attr('class', 'data-label')
                .attr('alignment-baseline', 'middle')
                .attr('text-anchor', 'start');

              sideLabel.append('tspan')
                .attr('x', 0)
                .attr('dy', '0em')
                .classed('highlight', true)
                .text(d3.format(",.2r")(d.value));

              sideLabel.append('tspan')
                .attr('x', 0)
                .attr('dy', '1.3em')
                .text(function() {
                  if (d.name == 'United Kingdom' || 
                    d.name == 'Netherlands' || 
                    d.name == 'Czech Republic') {
                    return 'Hungarians work in the';
                  } else {
                    return 'Hungarians work in';
                  }
                });

              sideLabel.append('tspan')
                .attr('x', 0)
                .attr('dy', '1.2em')
                .classed('highlight', true)
                .text(d.name);
            }
          })
          .on('mouseleave', function(d) {
            d3.select('#marker_' + d.id).selectAll('circle')
            .classed('active', false);

            d3.selectAll('.data-insert').remove();
          })

          // Title
          titleContainer = map.append('g')
          .attr('transform', 'translate(20, 20)')

          title = titleContainer.append('text')
          .attr('x', 0)
          .attr('class', 'title')

          title.append('tspan')
          .attr('x', 0)
          .attr('dy', '0em')
          .attr('alignment-baseline', 'hanging')
          .text('Where Do Hungarian')

          title.append('tspan')
          .attr('x', 0)
          .attr('dy', '1.2em')
          .attr('alignment-baseline', 'hanging')
          .text('Expats Live?')

          title.append('tspan')
          .attr('x', 0)
          .attr('dy', '3em')
          .attr('alignment-baseline', 'hanging')
          .classed('subtitle', true)
          .text('Estimated number of Hungarians working in EU countries')

          titleContainer.selectAll('tspan')
          .style('opacity', 0)
          .transition()
          .style('opacity', 1)
          
          // Add legend
          const legendData = [
            {'label': '100k', 'value': 100000},
            {'label': '10k', 'value': 10000},
            {'label': '<1k', 'value': 1000}
          ];
          const legendPadding = 5;
          const legend = map.append('g').attr('id', 'legend');

          // Add marker circles to legend
          legend.selectAll('.marker')
          .data(legendData.sort((a, b) => a.value - b.value))
          .enter()
          .append('g')
            .classed('marker', true)  
            .attr('transform', (d, i, a) => {
              // Arrange marker circles to bottom baseline
              const maxRadius = radiusScale(legendData.
                sort((a, b) => b.value - a.value)[0].value)
              const currentRadius = radiusScale(d.value)
              let y = maxRadius * 2 - currentRadius + legendPadding
            
              // Arrange marker centers horizontally 
              // to have even spacing between circle edges
              let x = a.map(item => radiusScale(item.__data__.value)).slice(0, i + 1)
              .map((item, idx, arr) => {
                if (idx == arr.length - 1) {
                  return item;
                } else {
                  return item * 2;
                }
              })
              .reduce((p, c) => p + c + legendPadding, 0);

              return 'translate(' + x + ',' + y + ')';
            })
          .append('circle')
            .attr('r', d => radiusScale(d.value));

          // Add text labels to legend markers
          legend.selectAll('.marker')
          .append('text')
            .classed('legend-label', true)
            .text(d => d.label)
            .attr('y', d => radiusScale(d.value) + 3)
            .attr('alignment-baseline', 'hanging')
            .attr('text-anchor', 'middle');    
          
          // Set border around legend
          const legendDimensions = legend.node().getBBox();
          legend.append('rect').classed('legend', true)
            .attr('width', legendDimensions.width + legendPadding * 2)
            .attr('height', legendDimensions.height + legendPadding * 2)
            .attr('x', legendDimensions.x - legendPadding - 1)
            .attr('y', legendDimensions.y - legendPadding + 1);

          // Place legend at bottom
          legend
          .attr('transform',
            'translate(10,' + 
            (height - legendDimensions.height - 2 * legendPadding) +
            ')');

          legend
          .style('opacity', 0)
          .transition()
          .style('opacity', 1)

          // Reference
          map.append('g')
          .attr('transform', 'translate(' + (width - 3) + ',' + (height - 3) + ')')
          .append('text')
          .attr('alignment-baseline', 'baseline')
          .attr('text-anchor', 'end')
          .text('Data: Portfolio.hu')
          .attr('class', 'reference')
        });
      });
      })(window, document);
    </script>
  </div>

_(You might realize that some important countries are missing. Romania, Slovakia and Slovenia was deliberately left out by Portfolio.hu for the potential bias caused by the local Hungarian minorities.)_

I spent quite a few hours with this, partially because I never used `topoJSON` and `geoPath` before (these are needed to draw the shapes of the map), but mostly I was experimenting with the code for fun before settling with the final version.

It's not a mind-exploding super-exciting piece of data art, just some basic elements glued together, and there are endless possibilities in enhancing it. Still, it might convey the message better, or even draw some extra attention compared to the original version.

__Which one do you like more: the original bar chart or the map version? Let me know in the comments!__
