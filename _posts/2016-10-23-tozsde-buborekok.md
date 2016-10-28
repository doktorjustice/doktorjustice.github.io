---
title: Tőzsdebuborékok
date: 2016-10-28
tags: dataviz, d3.js
header-img: img/bubbles.jpg
subtitle: Házilag, ingyenes hozzávalókból.
---
A blog történetének (pláne az új blog történetének) legrosszabb címadásával igyekszem ellensúlyozni, hogy a beharangozás után két héttel jön a következő poszt (bölcs taktika). Igazából nem is _olyan_ tőzsdebuborékok ezek, csak egy kis adatvizualizáció `D3.js`-ben.

### Miről beszélnek az emberek?

A kísérlet célja a szemnek is tetszetős módon ábrázolni, melyik részvényekről folyik a legélénkebb diskurzus a piacon. Az ábrán minden buborék egy-egy részvény, a buborékok területe pedig azzal arányos, hogy a [Stocktwits](https://stoctwits.com) "trending" bejegyzéseiben hányszor említik a felhasználók. Minél többször fordul elő egy részvény, annál nagyobb és sötétebb a buborék.

<div>
    <script src="https://d3js.org/d3.v4.js"></script>
    <svg height="600"></svg>
    <div>
      <button id="refresh"><small>Újrarajzolás</small></button>
    </div>
    <script>
        (function(window, document) {
          'use strict';

          let refresh = document.getElementById('refresh');
          
          window.onload = function() {
            d3.json('https://dotkom.info/stocktwits/testdata', d => {
              extractData(d);
              refresh.addEventListener('click', () => extractData(d));
            });
          }

          function extractData(messages) {
            let data = {};
            messages.forEach(message => {
              if (message.symbols) {
                let symbols = message.symbols;
                symbols.forEach(symbol => {
                  if (data[symbol.symbol]) {
                    data[symbol.symbol].count += 1;
                  } else {
                    data[symbol.symbol] = {
                      count: 1,
                      title: symbol.title
                    };
                  }
                });
              }
            });
            drawPack(arrayify(data));

            let textData = {
              messages: messages.length,
              minDate: d3.min(messages, d => new Date(d.created_at)),
              maxDate: d3.max(messages, d => new Date(d.created_at))
            };
            addTexts(textData);
          }

          function arrayify(data) {
            let list = Object.keys(data);
            let array = list.map(item => {
              return {
                name: item,
                count: data[item].count,
                title: data[item].title
              };
            });
            return array;
          }

          function drawPack(data, textData) {
            // Remove bubbles and caption before creating any new
            d3.select('svg').selectAll('g').remove();
            d3.selectAll('.caption').remove();

            // Create hierarchy from data
            let nodes = {
              children: data
            };
            let root = d3.hierarchy(nodes)
            .sum(d => d.count);

            // Create pack layout
            let diameter = 600;
            let pack = d3.pack().size([diameter, diameter]).padding(2);

            // Color scale
            let circleColor = d3.scaleLog()
            .domain([d3.min(nodes.children, d => d.count),
              d3.max(nodes.children, d => d.count)])
            .range(['#9FC3FF', '#2F2BAD'])
            .interpolate(d3.interpolateHsl);

            // Scale for circles' movement
            let yScale = d3.scaleLog()
            .domain([d3.min(nodes.children, d => d.count),
              d3.max(nodes.children, d => d.count)])
            .range([10, 150]);

            // Root svg
            let svg = d3.select('svg')
            .attr('width', 900)
            .attr('height', diameter + 100);

            // Data enter
            let node = svg.selectAll('.node')
            .data(pack(root).leaves())
            .enter()
            .append('g')
            .attr('class', 'node')
            .attr('transform', d => 'translate(' + d.x + ',' + d.y + ')');

            // Add bubbles
            node.append('circle')
            .style('fill', d => circleColor(d.data.count))
            .attr('cy', d => yScale(d.data.count))
            .style('opacity', 0)
            .transition()
            .duration(600)
            .attr('cy', 0)
            .style('opacity', 1)
            .delay((d, i) => {
              let delay;
              if (d.r > 30) {
                delay = i * 10;
              } else {
                delay = i * 5;
              }
              return delay;
            })
            .ease(d3.easeBackOut)
            .attr('r', d => d.r)
            .call(endAll, attachListeners);

            // Wait for the end of all bubble transitions
            function endAll(transition, callback) {
              if (!callback) callback = function() {};
              if (transition.size === 0) {
                callback();
              }
              let n = 0;
              transition
              .each(() => ++n)
              .on('end', function() {
                if (!--n) {
                  callback.apply(this, arguments);
                }
              });
            }

            // Bubble titles
            node
            .append('text')
            .attr('dy', '.3em')
            .style('opacity', 0)
            .style('text-anchor', 'middle')
            .style('font-family', 'arial')
            .style('font-size', d => {
              return Math.floor(d.r / 20) + 10;
            })
            .style('fill', '#ffffff')
            .style('pointer-events', 'none')
            .filter(d => d.r > 20)
            .text(d => d.data.name)
            .transition()
            .duration(1000)
            .delay((d, i) => i * 40)
            .style('opacity', 1);

            // Events
            function attachListeners() {
              node.on('mouseover', function(d) {
                d3.select(this).select('circle')
                .style('stroke-width', 3)
                .style('stroke', 'orange');

                let legend = svg.append('g')
                .attr('class', 'legend')
                .attr('transform', 'translate(560, 30)');

                legend.append('text')
                .style('text-anchor', 'left')
                .style('font-family', 'arial')
                .style('font-size', 12)
                .style('fill', '#888888')
                .text(() => d.data.title);

                let legendText = legend.append('text')
                .attr('y', 22)
                .style('text-anchor', 'left')
                .style('font-family', 'arial')
                .style('fill', '#888888')
                .style('font-size', 12);

                legendText.append('tspan')
                  .style('font-weight', 'bold')
                  .style('font-size', 20)
                  .text(() => d.data.count);

                legendText.append('tspan')
                  .text(' mentions');
              });

              node.on('mouseout', function(d) {
                d3.select(this).select('circle')
                .transition()
                .duration(200)
                .style('stroke', 'none');

                svg.select('.legend').remove();
              });
            }
          }

          // Add caption
          function addTexts(textData) {
            let caption = `Cashtag mentions from ${textData.messages} 
            StockTwits trending messages between ${textData.minDate.toLocaleString()} 
            and ${textData.maxDate.toLocaleString()}.`;

            let svg = d3.select('svg');

            svg.append('text').attr('class', 'caption')
            .attr('transform', 'translate(' + 0 + ',' + (svg.attr('height') - 60) + ')')
            .attr('y', 50)
            .style('fill', '#888888')
            .style('opacity', 0)
            .style('font-size', '13')
            .transition()
            .duration(1300)
            .delay(1500)
            .ease(d3.easeCubicOut)
            .attr('y', 0)
            .style('opacity', 1)
            .text(caption);
          }
        })(window, document);
    </script>
</div>

>A Stocktwits a tőzsdéseknek szóló Twitter. A bejegyzésekben a hashtagekhez hasonlóan `$` jellel kezdődő ún. _cashtagekkel_ lehet jelölni az egyes részvényeket, pl. `$AAPL`-lel az Apple-t, `$MSFT`-vel a Microsoftot, stb. Ezek alapján is kereshetők, rendszerezhetők a posztok. Bár a Twitterre épülve indult a közösség, időközben külön platformmá nőtte ki magát, saját API-val. Ezt használtam én is.

Ezek egyelőre csak statikus múltbeli adatok, egy péntek éjjel és vasárnap délelőtt közötti időszakról, összesen 1530 bejegyzésből. (Vidd az egérkurzort a buborékra, és megmutatja a papír nevét és az említések számát is.) Pont ez volt az a hétvége amúgy, amikor bejelentették, hogy az AT&T (T) felvásárolja a Time Warnert (TWX), ez világosan átjön az ábráról.

### Mi értelme ennek az egésznek?

Igazi gyakorlati haszna így nem sok van, de... 

1. ...legalább született egy bejegyzés a Dotkomra. 
2. ...ez az első komplettebb D3.js próbálkozásom.
3. ...sok fejlesztési irány rejlik még benne. 
4. ...már ez a verzió is egész jól mutat.
5. ...legalább nagyon jót játszottam vele.

### Van hová fejleszteni

Még ezzel a kis aprósággal is sok egyebet lehetne kezdeni. Például:

* Lehet dinamikus, animálva mutatva az időbeli változásokat.
* Lehet valós idejű, mindig az aktuális diskurzusnak megfelelően alakítva a buborékfelhőt.
* Az egyedi buborékokról előugró extra információ is lehet sokkal bővebb.
* A legérdekesebb dimenzió az lenne, hogy az egyes részvényeket _melyik más papírokkal együtt_ emlegetik.

Ezekkel már egyébként lehetne akár gyakorlati haszna is. A Stocktwits API sajnos nem túl megengedő, egyszerre legfeljebb 30 üzenetet ad vissza ("lapozható" formában, azaz további kérésekkel több üzenethez lehet jutni), és óránként maximum 200 lekérdezést engedélyez (azaz max. 18 másodpercenként lehet zaklatni, ha egyenletesen akarom csinálni), azt is csak a _trending_ témákra, nem a teljes részvényszekcióra.

De biztosan lesz még folytatása ennek a miniprojektnek.

### Itt van hozzá a kód is
Ez a script kaparja ki az említések számát az API által visszaadott adatokból, és rajzolja ki a grafikont belőle.

{% gist somiandras/5fdb594ddbd86db1e9d79e3e3ade4775 %}