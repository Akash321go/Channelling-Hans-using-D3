This is a visualisation created using D3 which replicates Hans Rosling's famous bubble graph visualisation. Hans Rosling is a data visualisation legend. His 2006 TED talk, The Best Stats You’ve Ever Seen, is one of the most viewed videos on the TED website (http://bit.ly/2doLzAY). Here is a link to my live visualisation. https://akash321go.github.io/Channelling-Hans-using-D3/ 


![Demo Picture](https://akash321go.github.io/Channelling-Hans-using-D3/Xot.png)


<!DOCTYPE html>
<html>
<head>


  <meta charset="utf-8">
  <title>Tribute to Mr.Hans Rosling</title>
  <script src="//cdnjs.cloudflare.com/ajax/libs/d3/3.5.5/d3.min.js"></script>
  <script type="text/javascript" src="https://code.jquery.com/jquery-3.1.1.js"></script>
 
  <style type="text/css">
  .axis path,
  .axis line {
    fill: none;
    stroke: black;
    shape-rendering: crispEdges;
    opacity: 0.7;
  }
  body{
    background-color: #B8B8B8;
  }
  .axis text {
    color: green;
    font-family: sans-serif;
    font-size: 11px;
  } 
  div.label_content {
    position: absolute;
    text-align: center;
    width: 150px;
    height: 80px;
    padding: 2px;
    font: 12px sans-serif;
    background: lightsteelblue;
    border: 0px;
    border-radius: 8px;
    pointer-events: none;
  }
  .countrynames{
    visibility:hidden;
  }
  .grid line {
    stroke: lightgrey;
    stroke-opacity: 0.7;
    shape-rendering: crispEdges;
  }
  .grid path {
    stroke-width: 0;
  }
  .bar {
    fill: #800000;
    opacity: 0.8;
  }
  .button-bar {
    margin-left: 15px;
    cursor: pointer;
  }
  .selected {
    font-weight: bold;
  }
  .texty{
    font-family: sans-serif;
    font-size: 11px;
    position: absolute;
    border-radius: 5;
    left :100px
    top :100px;  
  }
}
</style>


</head>
<body>


  <div style="width: 1400px; ">
    <div class="mainBody" style="float:left">

      <h1><i> Part 1: Channelling Hans: World GDP(2007- 2017)</i></h1>
      <p><b><i>Features of the visualisation:</i></b><p/>

<ul>
    <li><i>To start the visualisation, click on the start button to animate the changes in country statistics throughout the years. To pause, click on the pause button.</i></li>
    <li><i>To view data for a particular country and for a particular year, hover over the bubble representing the countries. It displays the name of the country, The GDP data, Global Competitiveness Index data, Region and Population.</i></li>
    <li><i>Slider feature to change the values through years 2007-2017. Click and drag the slider to change years manually.</i></li>
    <li><i>To view the trace of a country over the years in static view, click on the pause button and then click on the particular country. To continue the animation, click on the start button.</i></li>
    
</ul>


      <script type="text/javascript" id = "demo_code">
//Define margins
var margin = {top: 30, right: 30, bottom: 70, left: 80};
//width and height
var outerWidth = 1350;
var outerHeight = 480;
var svg_width  = 900  - margin.left - margin.right;
var svg_height = 500 - margin.top  - margin.bottom;
var rMin = 2; // "r" stands for radius
var rMax = 50;
var colorColumn = "area";
//year to display
var current_year = 2007;
var country="";
// define a function that filters data by year
function yearFilter(value){
  return (value['Year'] == current_year)
}
function countryFilter(value){
  return (value['Country']==country)
}
function countriesFilter(value) {
  if (showLineCountries.indexOf(value['Country']) != -1) {
    return true;
  } else {
    return false;
  }
}
var svg = d3.select("body").append("svg")
.attr("width",  outerWidth)
.attr("height", outerHeight)
.style("background", "white")
.append("g")
.attr("transform", "translate(" + margin.left + "," + margin.top + ")")
.attr("class", "nodes");
function make_x_gridlines() {   
  return d3.axisBottom(x)
  .ticks(5)
}
// gridlines in y axis function
function make_y_gridlines() {   
  return d3.axisLeft(y)
  .ticks(5)
}    
var traceCanvas=svg.append("g");    
//scaling 
var xScale = d3.scale.log().range([0, svg_width]);
var yScale = d3.scale.linear().range([svg_height, 0]);
var rScale = d3.scale.sqrt().range([2, 50]);
var colorScale = d3.scale.category10();
var clrscle = d3.scale.ordinal().range(["red","yellow","blue","green","rgb(25,25,112)","purple","gray"]);   //Define color to various regions.
//Define y-axis
var yAxis = d3.svg.axis()
.scale(yScale)
.ticks(7)
.orient("left");
//Define x-axis
var xAxis = d3.svg.axis()
.scale(xScale)
.ticks(10,d3.format(",d"))
.orient("bottom");
//current_year display 
svg.append("text")
.attr("id", "newyear")
.attr("text-anchor","end")         
.attr("x",800) 
.attr("y",390)
.attr("font-family", "Impact")
.style("opacity",0.2)
.attr("font-size","175px")
.text(+current_year);
//y-axis label
svg.append("text")
.attr("transform", "rotate(-90)")
.attr("y",-50 )
.attr("x",-200)
.attr("font-family", "Impact")
.attr("font-size","20px")
.style("text-anchor", "middle")
.attr("dy", "1em")
.attr("fill","black")
.text("Global Competitiveness Index");
// x axis label
svg.append("text")      
.attr("x", outerWidth/2 -80)
.attr("y",  svg_height + 35 )
.attr("font-family", "Impact")
.attr("font-size","20px")
.style("text-anchor", "middle")
.attr("fill","black")
.text("Gross Domestic Product");
//Legends
svg.append("rect")
.attr("x", 825)
.attr("y", 200)
.attr("width", 20)
.attr("height", 20)
.attr("fill","rgb(25,25,112)");
svg.append("rect")
.attr("x", 825)
.attr("y", 170)
.attr("width", 20)
.attr("height", 20)
.attr("fill","green");
svg.append("rect")
.attr("x", 825)
.attr("y", 140)
.attr("width", 20)
.attr("height", 20)
.attr("fill","blue");
svg.append("rect")
.attr("x", 825)
.attr("y", 110)
.attr("width", 20)
.attr("height", 20)
.attr("fill","yellow");
svg.append("rect")
.attr("x", 825)
.attr("y", 80)
.attr("width", 20)
.attr("height", 20)
.attr("fill","red");
svg.append("rect")
.attr("x", 825)
.attr("y", 230)
.attr("width", 20)
.attr("height", 20)
.attr("fill","purple");
svg.append("rect")
.attr("x", 825)
.attr("y", 260)
.attr("width", 20)
.attr("height", 20)
.attr("fill","gray");      
//Legend Text
svg.append("text")      
.attr("x", 950)
.attr("y",94 )
.style("text-anchor", "middle")
.attr("fill","black")
.text("Emerging and Developing Asia");
svg.append("text")      
.attr("x", 979)
.attr("y",125 )
.style("text-anchor", "middle")
.attr("fill","black")
.text("Middle East, North Africa, and Pakistan");
svg.append("text")      
.attr("x", 918)
.attr("y",156 )
.style("text-anchor", "middle")
.attr("fill","black")
.text("Advanced economies");
svg.append("text")      
.attr("x", 955)
.attr("y",187 )
.style("text-anchor", "middle")
.attr("fill","black")
.text("Latin America and the Caribbean");
svg.append("text")      
.attr("x", 970)
.attr("y",218 )
.style("text-anchor", "middle")
.attr("fill","black")
.text("Commonwealth of Independent States");
svg.append("text")      
.attr("x", 914)
.attr("y", 250 )
.style("text-anchor", "middle")
.attr("fill","black")
.text("Sub-Saharan Africa");
svg.append("text")      
.attr("x", 960)
.attr("y",278 )
.style("text-anchor", "middle")
.attr("fill","black")
.text("Emerging and Developing Europe");      
var filtered_dataset,filteredYearCountryData, xScale, yScale, zScale, xAxis, yAxis, xValue, yValue, xScaleExtra, yScaleExtra, animation, labelInfo,color,intervalId;
//Function to generate visualization
function Visualize(){
  d3.selectAll(".trace-nodes").remove();
filtered_dataset = dataset.filter(yearFilter);  //year wise filter of dataset
minGdp = 50;
maxGdp = 200000;
minComp = 2;
maxComp = 6;
xScale.domain([minGdp, maxGdp])  //extent function sets domain from min-max                      
yScale.domain([minComp, maxComp])                         
clrscle.domain(["Emerging and Developing Asia","Middle East, North Africa, and Pakistan","Advanced economies","Latin America and the Caribbean","Commonwealth of Independent States","Sub-Saharan Africa","Emerging and Developing Europe"]);  
svg.append("g")  //create a group of svg
.attr("class", "axis")
.style("background", "white")
.call(yAxis);
svg.append("g")
.attr("class", "axis")
.attr("transform", "translate(0," + svg_height+ ")")
.style("background", "white")
.call(xAxis);
rScale.domain(d3.extent(dataset, function (d){ return +d["Population"]; })); // circles according to population
var circles = svg.selectAll("circle").data(filtered_dataset,function key(d){
  return d.Country
});
// if(d.Global_Competitiveness_Index!=null ){
circles     //handle updation of circles
.transition()
.duration(1500)
.attr("cx", function (d){ return xScale(+d["GDP"]); })
.attr("cy", function (d){ return yScale(+d["Global_Competitiveness_Index"]); })
.attr("r",  function (d){ return rScale(+d["Population"]); })
.attr("fill",    function (d){ return   clrscle(d["Region"]); })
.attr("stroke",'black')
.style("opacity", function (d){ if(d["Global_Competitiveness_Index"]==0 || d["GDP"]<102){ return 0} else {return 0.7}});
circles  //creation of circles
.enter()
.append("circle")
.attr("cx", function (d){ return xScale(+d["GDP"]); })
.attr("cy", function (d){ return yScale(+d["Global_Competitiveness_Index"]); })
.attr("r",  function (d){ return rScale(+d["Population"]); })
.attr("fill",    function (d){ return   clrscle(d["Region"]); })
.attr("stroke",'black')
    //.style("opacity", 0.9);
    .on("mouseover", function(d){  
      labelInfo = d3.select(".mainBody")
      .append("div")
      .attr("class", "label_content")
                            .style("opacity", 0); //handles on-click event.
                            d3.select(this)
    .style("stroke-width", 2)  //highlight circle on mouseover
    .transition()
    .duration(500);
    labelInfo.style("opacity", 1.1);
    labelInfo.html("Country:"+d.Country
     +"<br/>"+"Region:"+d.Region
     +"<br/>"+"GCI:"+d.Global_Competitiveness_Index
     +"<br/>"+"population:"+ d.Population
     +"<br/>"+"GDP:"+ d.GDP)
    .style("left", (d3.event.pageX) + "px")
    .style("top", (d3.event.pageY - 28) + "px");
    
  })
 .on("mouseout", function(d){   //handles mouse-out event
   labelInfo.remove();
 })
 .on("click",function(d,i){
                //the trace will only be displayed when the chart is static
                labelInfo.remove();
                pauseAnimation();
                country=d.Country;
               //filter the country
               var cdata=dataset.filter(countryFilter);
               console.log(intervalId);
               var countryPoints = d3.select(".nodes").selectAll("circle");
               countryPoints.style("opacity",0.1);
              xValue = cdata.map(function(d){return d.GDP});
              yValue = cdata.map(function(d){return d.Global_Competitiveness_Index});
              zValue = cdata.map(function(d){return d.Population});
              color = cdata.map(function(d){return clrscle(d["Region"]); })
              console.log(color);
              for (var i = 0; i < xValue.length; i++) {
                d3.select(".nodes")
                .append("circle")
                .attr("r",rScale(zValue[i]))
                .attr("cx", xScale(xValue[i]))
                .attr("cy", yScale(yValue[i]))
                .style("fill", color[0])
                .text(d.Country)
                .style("opacity", 0.8)
                .style("stroke", "black")
                .style("stroke-width", "0.5")
                .attr("class", "trace-nodes");
              }
              d3.select(".nodes")
              .append("text")
              .text(d.Country)
              .attr("x", xScale(xValue[1]))
              .attr("y", yScale(yValue[1]))
              .attr("class", "trace-nodes")
              .style("font-size", "15pt");
              if(intervalId==1){
                startAnimation();
              }
            })
circles.exit()  //handle exiting circle
.transition()
.duration(1000)
       //.attr("r",0)
       .style("opacity",0.3)
       .remove();
       d3.select("#year_header").text("Year: " + current_year)
       d3.select("#newyear").text(current_year)
     }
     function startAnimation() {
      if(intervalId ==1) {
        intervalId = window.setInterval(animation ,1200);
      }
    }
    function pauseAnimation() {
      if(intervalId != null) {
        console.log("pause");
        window.clearInterval(intervalId);
      }
    }
d3.csv("GCI_CompleteData4.csv", function(data){   //access csv data
  dataset = data;
  Visualize();
});
</script>
</div>
</div>
<br>
<!--  -->
<div class = "button-bar" style="display: inline; padding-left: 2%;">   
  <a id = "play_button"><img src="play.png" style="height: 32px; width: 32px"></a>
  <a id = "pause_button"><img src="pause.png" style="height: 32px; width: 32px"></a>          
</div>
<!-- slider creation -->
<div style="display: inline;">  
  <input class = "slider" type="range" Id="Year" value = 2007 min="2007" max="2017" oninput = "sliderchange(Year.value)" style="width: 800px">
</div>

<script type="text/javascript">
    
    document.getElementById("pause_button").style.display = "none";
    d3.select("#play_button")
    .on("click", function() {
    // Set up the interval callback
    playInterval = setInterval(function() {  //increament current_year 
      current_year++;
      $("#Year").val(current_year);      
    if(current_year > 2017){   //loop creation
      current_year = 2007;
    }
    Visualize();
    document.getElementById("play_button").style.display = "none";
    document.getElementById("pause_button").style.display = "inline";
  }, 1000);
  }); 
    var playInterval;      
     d3.select("#pause_button")  //play-button
     .on("click", function() {            
      clearInterval(playInterval);
      document.getElementById("play_button").style.display = "inline";
      document.getElementById("pause_button").style.display = "none";
    }); 
  function sliderchange(year)  //move slider with current_year
  {
    current_year= year
    Visualize();
    d3.select("#year_header").text("Year: " + year)
  }
</script>

</div>
</div>


<h1><i> Part 2: Extending Hans</i></h1>
<p><b><i>Features of the visualisation:</i></b><p/>
  <li><i>The second visualisation shows a bar chart representation used to compare the 12 pillars of Global Competitiveness Index.</i></li>
  <li><i>The values for the pillars are shown as a cumilative average throughout the years(2007-2017). </i></li>
  <li><i> Click on the dropdown and select the country for which you wish to view the data.</i></li>



<div id='vis-container'></div>
<div id = "countries"></div>
<div id= "year"></div>
<!-- <h3 id = "year_header">Year: 2007</h3> -->
<script type="text/javascript">
          // Load and munge data, then make the visualization.
          var fileName = "GCI_CompleteData4.csv";
          var pillars = ["1st_pillar_Institutions", "2nd_pillar_Infrastructure", "3rd_pillar_Macroeconomic_environment", "4th_pillar_Health_and_primary_education", "5th_pillar_Higher_education_and_training","6th_pillar_Goods_market_efficiency", "7th_pillar_Labor_market_efficiency", "8th_pillar_Financial_market_development", "9th_pillar_Technological_readiness","10th_pillar_Market_size","11th_pillar_Business_sophistication_","12th_pillar_Innovation"];
var filtered_data;
d3.csv(fileName, function(error, data) {
  dataset= data;
  var map = {};
  var map1 = {};
  data.forEach(function(d) {
    var country = d.Country;
    var year = d.Year;
    map[country] = [];
    map1[year] = [];
    pillars.forEach(function(field) {
      map[country].push( +d[field] );
    });
  });
    makeVis(map);
  // setInterval(function() {
  //   current_year = current_year + 1;
  //   if(current_year > 2017){
  //     current_year = 2007;
  //   }
  //   makeVis(map);
  // }, 2000);
});
var makeVis = function(map) {
              // Define dimensions of vis
              var margin = { top: 30, right: 50, bottom: 150, left: 50 },
              width  = 900 - margin.left - margin.right,
              height = 650 - margin.top  - margin.bottom;
              
              filtered_data = dataset.filter(yearFilter);
              // Make x scale
              var xScale = d3.scale.ordinal()
              .domain(pillars)
              .rangeRoundBands([0, width], 0.1);
              // Make y scale, the domain will be defined on bar update
              var yScale = d3.scale.linear()
              .range([height, 0]);
              // Create canvas
              var canvas = d3.select("#countries")
              .append("svg")
              .attr("width",  width  + margin.left + margin.right)
              .attr("height", height + margin.top  + margin.bottom)
              .append("g")
              .attr("transform", "translate(" + margin.left + "," + margin.top + ")");
              // Make x-axis and add to canvas
              var xAxis = d3.svg.axis()
              .scale(xScale)
              .orient("bottom");
              canvas.append("text")      
              .attr("transform", "rotate(-90)")
              .attr("y",-50 )
              .attr("x",-200)
              .attr("font-family", "Impact")
              .style("text-anchor", "middle")
              .attr("dy", "1em")
              .attr("font-size","20px")
              .attr("fill","black")
              .text("Values");
              canvas.append("text")      
              .attr("x", 410)
              .attr("y",  620)
              .attr("font-family", "Impact")
              .style("text-anchor", "middle")
              .attr("font-size","20px")
              .attr("fill","black")
              .text("Pillars of Global Competitiveness Index");
              canvas.append("g")
              .attr("class", "x axis")
              .attr("transform", "translate(0," + height + ")")
              .call(xAxis)
              .selectAll("text")
              .style("text-anchor", "end")
              .attr("dx", "-.8em")
              .attr("dy", ".15em")
              .attr("transform", "rotate(-35)");
              // Make y-axis and add to canvas
              var yAxis = d3.svg.axis()
              .scale(yScale)
              .orient("left");
              var yAxisHandleForUpdate = canvas.append("g")
              .attr("class", "y axis")
              .call(yAxis);
              yAxisHandleForUpdate.append("text")
              .attr("transform", "rotate(-90)")
              .attr("y", 6)
              .attr("dy", ".71em")
              .style("text-anchor", "end");
              var updateBars = function(data) {
                  // First update the y-axis domain to match data
                  yScale.domain( d3.extent(data) );
                  yAxisHandleForUpdate.call(yAxis);
                  var bars = canvas.selectAll(".bar").data(data);
                  // Add bars for new data
                  bars.enter()
                  .append("rect")
                  .attr("class", "bar")
                  .attr("x", function(d,i) { return xScale( pillars[i] ); })
                  .attr("width", xScale.rangeBand())
                  .attr("y", function(d,i) { return yScale(d); })
                  .attr("height", function(d,i) { return height - yScale(d); });
                  // Update old ones, already have x / width from before
                  bars
                  .transition().duration(1000)
                  .attr("y", function(d,i) { return yScale(d); })
                  .attr("height", function(d,i) { return height - yScale(d); });
                  // Remove old ones
                  bars.exit()
                  .transition()
                  .duration(1000)
                  .ease(d3.easeBounce)
                  .style("fill", "red")
                  .remove();
                  //d3.select("#year_header").text("Year: " + current_year)
                };
              // Handler for dropdown value change
              var dropdownChange = function() {
                var newCountry = d3.select(this).property('value'),
                newData   = map[newCountry];
                console.log(newData);
                updateBars(newData);
              };
              
              var countries = Object.keys(map).sort();
              // var years = Object.keys(map1).sort();
              var dropdown = d3.select("#countries")
              .insert("select", "svg")
              .on("change", dropdownChange);
 
              dropdown.selectAll("option")
              .data(countries)
              .enter().append("option")
              .attr("value", function (d) { return d; })
              .text(function (d) {
                      return d[0].toUpperCase() + d.slice(1,d.length); // capitalize 1st letter
                    }); 
               var initialData = map[ countries[0] ];
               updateBars(initialData);
             };
           </script>
         </body>
         </html>
      

Features of the Hans Rosling Visualisation:

1. A bubble plot visualisation which represents the countries of the world
2. Countries of the world described by GDP and Global Competitive Index mapped to x and y axis position.
3. The population of each country mapped to bubble area. As the years slide through, the size of the bubble changes as per the country population figure .
4. To start the visualisation, press on the start button to animate the changes in country statistics throughout the years. To pause, press on the pause button.
5. To view data for a particular country and for a particular year, hover over the bubble representing the countries. It displays the name of the country, The GDP data, Global Competitiveness Index data,   Region and Population.
6. Each Region is assigned to a particular colour used to differentiate between the regions and the legend for the same is represented on the righthand side of the chart.
7. Slider feature to change the values through years 2007-2017. Click and drag the slider to change years manually.
8. To view the trace of a country over the years in static view, press on the pause button and then click on the particular country. To continue the animation, press on the start button.
9. The second visualisation shows a bar chart representation used to compare the 12 pillars of Global Competitiveness Index.
10. The values for the pillars are shown as a cumilative average throughout the years(2007-2017).
11. Click on the dropdown and select the country for which you wish to view the data.
