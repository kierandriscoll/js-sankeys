# D3

D3 is a javascript library that makes it easier to create visualisations in a web browser.
You need to have some understanding of HTML, CSS, and Javascript

# HTML & CSS Recap
HTML is made up of elements. Elements usually start and end with a tag. Elements can have attributes. Eg. the element *<style>* can have the attribute *type*. Elements can be nested inside each other, and so can have a parent/child heirarchy.
  
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

## CSS
CSS consists of selectors and rules. Selectors are the names of the HTML elements or classes that the styles will apply to; Rules are defined within curly brackets { }  

p { font-family: sans-serif;
    color: lime;
  }

## SVG
D3 creates visualisations by using the <SVG> element (Scalable Vector Graphics) - this draws shapes (eg. circles, rectangles, lines etc..) based on given parameters. In raw HTML this looks like:

<svg width="100" height="100">
    <circle cx="50" cy="50" r="20"  fill="orange" stroke="gray" stroke-width="2"/>
    <rect x="10" y="10" width="50" height="50" fill="lime" stroke-width="4" stroke="pink" />
    <line x1="20" y1="40" x2="90" y2="90" stroke="blue" stroke-width="4" />
</svg>


# Javascript Recap
Variables can be defined and can contain numbers or strings.
var = 123;

An *array* can be created [ ]. Values can be referenced using the position in the array eg. cars[0] = Saab
var cars = ["Saab", "Volvo", "BMW"];

*functions* can created using as shown below:
function myFunction() { alert( cars[1] ); }

An *object* can created using { }, and are made of key:value pairs. Values can be numbers, strings, arrays, objects or functions. A value can be referenced using the object and key name separated by a period eg. people.lastName = Doe
var people = {firstName:"John", lastName:"Doe"};     


# D3 Explained
The basic D3 code below finds the <body> element and inserts an <svg> element inside it: 
d3.select("body").append("svg");

The D3 library is an object made up of lots of functions. *select* is a one of these functions (nb. functions that are part of an object are referred to as *methods*). D3 lets you chain its functions/methods using the . eg d3.method1().method2().method3()    

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


# Common D3 methods
The function for reading CSV files: 
d3.csv("dataset.csv",
       function(error, mydata) {  }
)

