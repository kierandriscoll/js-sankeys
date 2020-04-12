# D3

D3 is a javascript library that makes it easier to create visualisations in a web browser.
You need to have some understanding of HTML, CSS, and Javascript

# HTML & CSS Recap
HTML is made up of elements. Elements usually start and end with a tag. Elements can have attributes. Eg. the element **<style>** can have the attribute **type**. Elements can be nested inside each other, and so can have a parent/child heirarchy.

```html  
<!DOCTYPE html>
  <head>
    <style type="text/css">
   
    </style>
    <title>D3 Guide</title>
  </head>
  <body>
    <h1>Page One Heading</h1>
    <p>Paragraph of text.</p>
  </body>
```

## CSS
CSS consists of selectors and rules. Selectors are the names of the HTML elements or classes that the styles will apply to; Rules are defined within curly brackets { }  
```css
p { font-family: sans-serif;
    color: lime;
  }
```
**Bootstrap** is a framework that includes a lot of pre-set CSS classes, and that allows you to easily use a grid system.

## SVG
D3 creates visualisations by using the <SVG> element (Scalable Vector Graphics) - this draws shapes (eg. circles, rectangles, lines etc..) based on given parameters. In raw HTML this looks like:   

```html
<svg width="100" height="100">
   <circle cx="50" cy="50" r="20"  fill="orange" stroke="gray" stroke-width="2"/>
   <rect x="10" y="10" width="50" height="50" fill="lime" stroke-width="4" stroke="pink" />
   <line x1="20" y1="40" x2="90" y2="90" stroke="blue" stroke-width="4" />
</svg>
```

# Javascript Recap
Variables can be defined and can contain numbers or strings.
  var = 123;

An **array** can be created [ ]. Values can be referenced using the position in the array eg. cars[0] = Saab
```js
var cars = ["Saab", "Volvo", "BMW"];
```

**functions** can created using as shown below:
```js
function myFunction() { alert( cars[1] ); }
```
Alternatively you may be able to use arrow functions:
```
myFunction = () => { alert( cars[1] ); }
```

An **object** can created using { }, and are made of key:value pairs. Values can be numbers, strings, arrays, objects or functions. A value can be referenced using the object and key name separated by a period eg. people.lastName = Doe
```js
var people = {firstName:"John", lastName:"Doe"};     
```

# D3 Explained
The basic D3 code below finds the <body> element and inserts an <svg> element inside it: 
  d3.select("body").append("svg");

The D3 library is an object made up of lots of functions. **select** is a one of these functions (nb. functions that are part of an object are referred to as *methods*). D3 lets you chain its functions/methods using the . eg d3.method1().method2().method3()    
```js
var svg = d3.select("body")
            .append("svg")
            .attr("width", 1500)
            .attr("height", 1500);
                   
svg.append("g")
   .attr("class", "x axis")
   .attr("transform", "translate(0," + plotheight + ")") 
   .call(xAxis)
   .append("text")
   .attr("class", "label")
   .attr("x", plotwidth /2 )
   .attr("y", margin.bottom )
   .text(“Date”);                    
```

## Common D3 methods
The function for reading CSV files:  
```js
d3.csv("dataset.csv",
       function(error, mydata) {  }
)
```

# JQuery
JQuery is a javascript library that makes it easier to change a HTML document. The main way of selecting a tag/id/class.. with JQuery is to use the **$** function with the relevant CSS selector - for example the code below selects all HTML tags whose id="mapid" :
```js
$("#mapid");
```
It can also be used to create new tags (which will need to be inserted later).
```js
$('<p class="greet">Hello!</p>');
```
The **DataTables** library depends on JQuery so that it can be interactive (sortable/searchable/pagination..). 



# From R to Javascript

## UI & HTML
A R Shiny dashboard user interface usually contains a header, sidebar and body. A basic body might look like:
```r
ui_body <- dashboardBody(
  
      fluidRow(
        box(width = 6,
            solidHeader = TRUE,
            status = "primary",
            title = "Plotly Scatter",
            
            plotly::plotlyOutput(outputId = "plotly_line")
            
        ),
        
        DT::DTOutput(outputId = "data_table")
      )
)
```
R Shiny is based AdminLTE/Bootstrap, and the R code above generates the following HTML and CSS classes:
```html
<div class="content-wrapper">
  <section class="content">
    <div class="row">
      <div class="col-sm-6">
        <div class="box box-solid box-primary">
          <div class="box-header">
            <h3 class="box-title">Plotly Scatter</h3>
          </div>
          <div class="box-body">
            <div id="plotly_line" style="width:100%; height:400px; " class="plotly html-widget html-widget-output"></div>
          </div>
        </div>
      </div>
      <div id="data_table" style="width:100%; height:auto; " class="datatables html-widget html-widget-output"></div>
    </div>
  </section>
</div>
```




## A DataTable in R
```r
library(dplyr)

tabledata <- data.frame("Category" = c("AAAAAAAA","BBBBBBB","CCCCCCCCC","DDDDDDDD","EEEEEEE"),
                        "Comments" = c(542,246,658,254,336),
                        "pct" = c(0.25,0.12,0.3,0.12,0.21),
                        "NetEasy"= c(55,32,-10,28,-25),
                        "AbleToDo" = c(0.9,0.85,0.2,0.6,0.1),
                        stringsAsFactors = F) 


DT::datatable(data = tabledata,
              colnames = c("Category", "Comments", "%", "Net Easy", "Able To Do"),
              selection = "single",
              options = list(dom = "t",
                             lengthMenu = c(15, 25, 50),
                             searchHighlight = TRUE,
                             columnDefs = list(list(targets = c(0), width = "40%"),
                                               list(targets = c(1,2,3,4), width = "15%")
                             )
              ),
              rownames = FALSE,
              escape = FALSE) %>%
DT::formatCurrency("Comments",
                   currency = "",
                   interval = 3,
                   mark = ",",
                   digits = 0) %>%
DT::formatPercentage(c("AbleToDo", "pct"), 0) %>%
DT::formatStyle(c("pct", "AbleToDo"),
                background = DT::styleColorBar(c(0,1),
                                               'rgba(55,126,34,0.7)'),
                backgroundSize = '98% 88%',
                backgroundRepeat = 'no-repeat',
                backgroundPosition = 'center') %>%
DT::formatStyle("NetEasy",
                fontWeight = "bold",
                color = DT::styleInterval(c(0),
                                          c("red", "green")))  
```

For use in Javascript the dataframe needs to be converted to a JSON Array of Arrays
```r
library(jsonlite)
jsondata <- jsonlite::toJSON(setNames(tabledata,NULL))
```

## A Datatable in JS
```html
<div id="dt_container">
<table id="table_id" class="display"></table>
</div>
```
```js
<script>
var jsondata = `r jsondata`;

    $('#table_id').DataTable({
        data: jsondata,
        columns: [ { title: "Category" },
                   { title: "Comments" },
                   { title: "%" },
                   { title: "Net Easy" },
                   { title: "Able To Do" }
        ],
        columnDefs: [ { targets: [0], width: "40%"},
                      { targets: [1,2,3,4], width: "15%" },
		      { targets: [2,4], render: function ( data, type, row ) { return data*100 + "%"; } },
		      { targets: [3], createdCell: function(td, cellData, rowData, row, col) {
		      					var color = (cellData > 0) ? 'green' : 'red';
                                                        $(td).css('color', color);
							$(td).css('font-weight', 'bold');	} }
        ],
	dom: 't',
	lengthMenu: [5, 10, 15]
    });
</script>
```
DataTable relies on JQuery to work, and has some other methods that can update the table when an event occurs. These methods can be chained: 
```
$('#table_id').DataTable().clear();  // Removes all data from the table
$(#table_id).DataTable().rows.add(...);  // Adds multiple rows of data (must be supplied)
$(#table_id).DataTable().draw();  // Instruction to draw the table
$(#table_id).DataTable().clear().rows.add(...).draw();  // All methods chained together
```

## A Plotly Bar Chart in R
```r
plotly::plot_ly(data = tabledata,
                x = ~pct,
                y = ~Category,
                type = "bar",
                hoverinfo = 'text',
                text = ~paste(scales::percent(tabledata$pct, accuracy = 1), " of comments",
                              "<br>Category: ", tabledata$Category,
                              "<br>Able to Do:", scales::percent(tabledata$AbleToDo, accuracy = 1),
                              "<br>Net Easy:", scales::percent(tabledata$NetEasy, accuracy = 1))
) %>%
plotly::layout(title = paste("Category Distribution"),
               xaxis = list(title = "% in each category",
                            showgrid = FALSE,
                            showline = TRUE,
                            zeroline = FALSE,
                            tickformat = "%"),
               yaxis = list(title = "", showgrid = FALSE, showline = FALSE),
               margin = list(t = 25, l=100, r=25, b=60)) %>%
plotly::config(displayModeBar = FALSE)
```

For use in Plotly Javascript the columns of the dataframe needs to be separated in separate vectors/arrays
```r
library(jsonlite)
category <- jsonlite::toJSON(tabledata$Category)
comments <- jsonlite::toJSON(tabledata$Comments)
pct <- jsonlite::toJSON(tabledata$pct)
abletodo <- jsonlite::toJSON(tabledata$AbleToDo)
```

## Plotly Bar in JS
```html
<div id="plotly_bar_id" style="width:627px;></div>
```
```js
<script>
var setup = [{ x: `r pct`,
               y: `r category`,
               type: 'bar',
               orientation: 'h'
            }];
            
var layout = {title: 'Category Distribution',
              xaxis: { title: "% in each category",
                       showgrid: false,
                       showline: true,
                       zeroline: false,
                       tickformat: "%"  },
              yaxis: { title: "",
                       showgrid: false,
                       showline: false },
              margin: {t: 25, l: 100, r: 25, b: 60}
              };

Plotly.newPlot('plotly_bar_id', setup, layout, {displayModeBar: false});
</script>
```
