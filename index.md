---
title       : Retirement savings decay
subtitle    : A shiny sales pitch
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [quiz, bootstrap]            # {mathjax, quiz, bootstrap}
ext_widgets : {rCharts: [libraries/nvd3]}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
--- &radio

## Why use the retirement savings decay application?

1. To understand how much money you need to retire.
2. To plan out your future retirement.
3. Because it is super cool.
4. _All of the above!_

*** .hint
Not 1, 2, or 3.

*** .explanation
It is clearly all of the above...I can't believe you needed an explanation for this!

--- .class #id

## First you enter in the parameters as shown below:

![Inputs](./assets/img/inputs.jpg)


--- .class #id

## Then the algorithm does its thing.


```r
years <- 1:30; remains <- 325000; monthlyRev <- 2500; monthlyCost <- 5000; 
inflation <- 3; ror <- 8; remaining <- numeric()
for( i in years) {
        for( j in 1:12) {
            remains <- (remains + monthlyRev - monthlyCost*(1 + inflation/1200)) * (1 + ror/1200)
        }
        remaining[i] <- remains
}
df <- data.frame(year = years, remaining = remaining)
message <- "You can retire safely!"
if(min(df$remaining) <= 0) {
        dest <- min(df$year[df$remaining <= 0])
        message <- paste("You will be destitute in year", dest)
}
message
```

```
## [1] "You will be destitute in year 25"
```



--- .class #id

## Also, a chart is created.


<div id = 'chart1' class = 'rChart nvd3'></div>
<script type='text/javascript'>
 $(document).ready(function(){
      drawchart1()
    });
    function drawchart1(){  
      var opts = {
 "dom": "chart1",
"width":    800,
"height":    400,
"x": "year",
"y": "remaining",
"type": "multiBarChart",
"id": "chart1" 
},
        data = [
 {
 "year": 1,
"remaining": 320485.8643235 
},
{
 "year": 2,
"remaining": 315597.0576122 
},
{
 "year": 3,
"remaining":  310302.482355 
},
{
 "year": 4,
"remaining": 304568.4599627 
},
{
 "year": 5,
"remaining": 298358.5165397 
},
{
 "year": 6,
"remaining": 291633.1508754 
},
{
 "year": 7,
"remaining": 284349.5831779 
},
{
 "year": 8,
"remaining": 276461.4829536 
},
{
 "year": 9,
"remaining": 267918.6743011 
},
{
 "year": 10,
"remaining": 258666.8167437 
},
{
 "year": 11,
"remaining":  248647.059572 
},
{
 "year": 12,
"remaining": 237795.6674966 
},
{
 "year": 13,
"remaining": 226043.6152309 
},
{
 "year": 14,
"remaining": 213316.1484231 
},
{
 "year": 15,
"remaining": 199532.3081474 
},
{
 "year": 16,
"remaining": 184604.4159268 
},
{
 "year": 17,
"remaining": 168437.5160143 
},
{
 "year": 18,
"remaining": 150928.7713824 
},
{
 "year": 19,
"remaining": 131966.8095813 
},
{
 "year": 20,
"remaining": 111431.0143026 
},
{
 "year": 21,
"remaining": 89190.75814381 
},
{
 "year": 22,
"remaining":  65104.5716926 
},
{
 "year": 23,
"remaining": 39019.24364507 
},
{
 "year": 24,
"remaining": 10768.84623467 
},
{
 "year": 25,
"remaining": -19826.3202279 
},
{
 "year": 26,
"remaining": -52960.87041755 
},
{
 "year": 27,
"remaining": -88845.57193124 
},
{
 "year": 28,
"remaining": -127708.6859725 
},
{
 "year": 29,
"remaining": -169797.4193122 
},
{
 "year": 30,
"remaining": -215379.4967612 
} 
]
  
      if(!(opts.type==="pieChart" || opts.type==="sparklinePlus" || opts.type==="bulletChart")) {
        var data = d3.nest()
          .key(function(d){
            //return opts.group === undefined ? 'main' : d[opts.group]
            //instead of main would think a better default is opts.x
            return opts.group === undefined ? opts.y : d[opts.group];
          })
          .entries(data);
      }
      
      if (opts.disabled != undefined){
        data.map(function(d, i){
          d.disabled = opts.disabled[i]
        })
      }
      
      nv.addGraph(function() {
        var chart = nv.models[opts.type]()
          .width(opts.width)
          .height(opts.height)
          
        if (opts.type != "bulletChart"){
          chart
            .x(function(d) { return d[opts.x] })
            .y(function(d) { return d[opts.y] })
        }
          
         
        chart
  .showControls(false)
          
        chart.xAxis
  .axisLabel("Year")

        
        
        chart.yAxis
  .axisLabel("Retirement savings")
      
       d3.select("#" + opts.id)
        .append('svg')
        .datum(data)
        .transition().duration(500)
        .call(chart);

       nv.utils.windowResize(chart.update);
       return chart;
      });
    };
</script>

Kind of like the one above, but [way spiffier!](https://jamessul.shinyapps.io/dp_project)

