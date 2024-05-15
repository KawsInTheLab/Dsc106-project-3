<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";
  import worldData from "./world.json";

  let happinessData;

  onMount(async () => {
    const response = await fetch("./2022.csv");
    const csvData = await response.text();
    happinessData = d3.csvParse(csvData, d => {
      return { country: d.Country, happinessScore: +d.HappinessScore, rank: d.RANK };
    });
    renderGlobe(worldData, happinessData);
  });

  function debounce(func, wait, immediate) {
    let timeout;
    return function () {
      const context = this, args = arguments;
      const later = function () {
        timeout = null;
        if (!immediate) func.apply(context, args);
      };
      clearTimeout(timeout);
      timeout = setTimeout(later, wait);
      if (immediate && !timeout) func.apply(context, args);
    };
  }

  const handleMouseover = debounce(function (event, d) {
    const countryData = happinessData.find(h => h.country === d.properties.name);
    const rank = countryData ? countryData.rank : "Data Not Available";
    const happinessScore = countryData ? countryData.happinessScore : "Data Not Available";
    d3.select("#tooltip")
      .html(`Country: ${d.properties.name}<br>Rank: ${rank}<br>Happiness Score: ${happinessScore}`)
      .style("left", event.pageX + 10 + "px")
      .style("top", event.pageY - 28 + "px")
      .style("visibility", "visible");
  }, 500);

  function renderGlobe(data, happinessData) {
    const mapElement = document.querySelector("#map");
    const width = mapElement.getBoundingClientRect().width;
    const height = 500;
    let projection = d3.geoOrthographic()
      .scale(250)
      .center([0, 0])
      .rotate([0, -30])
      .translate([width / 2, height / 2]);
    let path = d3.geoPath().projection(projection);

  let svg = d3.select("#map").append("svg")
      .attr("width", width)
      .attr("height", height);
    svg.append("circle")
      .attr("cx", width / 2)
      .attr("cy", height / 2)
      .attr("r", 250)  // Match the projection's scale
      .attr("fill", "#f2f2f2");  // Light blue color for the ocean
    const legendScale = d3.scaleLinear().domain(d3.extent(happinessData, d => d.happinessScore)).range([0, 100]);

    const colorScale = d3.scaleSequential()
      .domain(d3.extent(happinessData, d => d.happinessScore))
      .interpolator(d3.interpolateHsl("red", "yellow"));

    svg.append("g")
      .attr("class", "countries")
      .selectAll("path")
      .data(data.features)
      .enter()
      .append("path")
      .attr("d", path)
      .attr("fill", d => {
        const countryData = happinessData.find(h => h.country === d.properties.name);
        return countryData ? colorScale(countryData.happinessScore) : "#ccc";
      })
      .style("stroke", "black")
      .style("stroke-width", 0.3)
      .style("opacity", 0.8)
      .on("mouseover", handleMouseover)
      .on("mouseout", () => d3.select("#tooltip").style("visibility", "hidden"));

    // Create a legend
    const legend = svg.append("g")
      .attr("class", "legend")
      .attr("transform", `translate(${width - 400}, 50)`);  // Position the legend

    const legendAxis = d3.axisRight(legendScale)
      .tickValues([d3.min(happinessData, d => d.happinessScore), d3.max(happinessData, d => d.happinessScore)])
      .tickFormat(() => ""); // Return empty strings for the ticks

    //legend.call(legendAxis);

    // Manually add "Happier" and "Unhappier" labels
    legend.append("text")
      .attr("x", 0) // Adjust x position
      .attr("y", -10) // Adjust y position for "Happier" (20 pixels higher)
      .attr("dy", "0.32em")
      .style("font-family", "'Gill Sans', serif")
      .text("Happier");

    legend.append("text")
      .attr("x", 0) // Adjust x position
      .attr("y", 110) // Adjust y position for "Unhappier" (20 pixels lower)
      .attr("dy", "0.32em")
      .style("font-family", "'Gill Sans', serif")
      .text("Unhappier");

    const defs = svg.append("defs");
    const linearGradient = defs.append("linearGradient")
      .attr("id", "gradient")
      .attr("x1", "0%")
      .attr("x2", "0%")
      .attr("y1", "100%")
      .attr("y2", "0%");

    linearGradient.selectAll("stop")
      .data(colorScale.ticks().map((t, i, n) => ({ offset: `${100*i/n.length}%`, color: colorScale(t) })))
      .enter().append("stop")
      .attr("offset", d => d.offset)
      .attr("stop-color", d => d.color);

    legend.append("rect")
      .attr("width", 20)
      .attr("height", 100)
      .style("fill", "url(#gradient)");

    // Drag functionality to rotate the globe
    svg.call(d3.drag().on("drag", (event) => {
        let rotate = projection.rotate();
        let k = 0.4; // Sensitivity factor
        let newLongitude = rotate[0] + event.dx * k;
        let newLatitude = rotate[1] - event.dy * k;

        // Clamping the latitude rotation to prevent flipping
        newLatitude = Math.max(-45, Math.min(45, newLatitude));

        projection.rotate([newLongitude, newLatitude]);
        svg.selectAll("path").attr("d", path);
    }));
  }
</script>

<div id="map" style="width: 100%;"></div>
<div id="tooltip" style="position: absolute; visibility: hidden; background-color: white; border: none; padding: 10px; border-radius: 5px; pointer-events: none; font-family: 'Gill Sans', serif; font-size: 14px;"></div>

