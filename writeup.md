# Writeup

### Vision
We decided to pursue an interactive visual that displays the happiness ranking and happiness score of different countries across the world.
We decided to do this because people often see rankings of countries’ data without understanding the global context. 
Certain geographical areas might have higher happiness levels, which could point to factors such as climate and hours of sunshine having influence over the happiness score.


### Visualization Design
We decided that the most effective visualization for answering this question would be one that implements a world map so that the viewer can make comparisons between countries based on their geographical context. We decided to create an interactive globe where the countries are colored based on their happiness index. In this case we have yellow being the happiest and red being the unhappiest since psychologically yellow is a happy color, and red can represent frustration and unhappiness. Since have a ranking scale instead of binary categories, we used a gradient to capture the representation of different scores. We created a legend so that the viewer can easily understand what the color and shading of the country represents. 


### Interaction Techniques
We chose a map that the viewer can drag around in order to rotate the globe and view all the countries. We also added tooltips so that the viewer can gain additional understanding of the country’s statistics, such as the name, ranking, and happiness score. Adding this information allows the viewer to more comprehensively understand the factors behind a country’s happiness ranking. We also fixed the globe so that it wouldn't flip over when rotating and dragging around so that the viewer doesn't get too disoriented and lose their placing when exploring the map. We also added a filter which would gray out the rest of the countries based on the aggregation. This means that if the viewer checked off Top 5, we would return the globe with only the Top 5 Happiest countries in color while the rest of the world is grayed out. This allows for quicker filters to explore countries and ranges based on the viewer's desire. 

### Design Choices and Teamwork
We decided to do a visualization using a map from the beginning, but we weren’t sure what our data set would be. Once we found the happiness dataset, we collaborated on how we wanted to approach the design aesthetics. With our dataset, we naturally gravitated towards using a globe design with which we found some inspiration on some d3 community forums. We also worked together to decide what additional information we would want to provide with the tooltips and our grouping filter. We liked the look of our Gill Sans seriff font so we used it throughout the page for the data displayed in tooltips to our title to our legend and filter.

### Division of Labor
**Niha**
- Suggested the map visualization and obtained the dataset containing the happiness levels of countries.
- Completed the write-up once the visualization had been finalized.
- Part of the brainstorming and visualization process

**Samuel**
- Worked on the data visualization of the globe through working on the legend, the drag effect, implementing the filter and centering the labels. Added some titles as well as other small design tweaks.
- Added to the writeup with some changes and tweaks.
- Part of the brainstorming and visualization process.

**Eduardo**
- Created the framework and baseline using svelte and d3 for the globe visualization.
- Manipulated the csv file to json to fit our needs.
- Implemented tooltips and draggability/rotation for our visualization.
- Part of the brainstorming and visualization process.

### Challenges
We mainly faced challenges with turning the data into an acceptable format for our visualization. We had to change it from a csv to a json format in order for it to render properly. Additionally, the actual implementation of the globe was confusing for a few reasons. This is because some tutorials online used tools that were banned such as plotly so it took us awhile to figure out how to make the happiness dataset work with our desired visualization. Another surprisingly difficult aspect was our legend. We couldn't get the "Unhappier" and "Happier" labels to not intersect with our gradient scale for the longest time. The filter was also quite difficult to implement at first but wasn't the most challenging issue out of the bunch. In totality, we probably spent around 12 hours per person to finish this, but if we add it up, maybe it would be around 20 hours. Honestly, the most time was spent in getting the framework for our globe implementation into svelte and finding data that would be valid and work well with our globe. Also just choosing a dataset took awhile but that's more human overthinking and pickiness.
