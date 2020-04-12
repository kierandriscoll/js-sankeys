# Javascript
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

# JSON Arrays
A JSON array is an **array of objects** and is used by various visualisation libraries. If you import tabular data into javascript it will normally need to be in this format:  
```js
var json = [{firstName:"John", lastName:"Doe"}, {firstName:"Susan", lastName:"Smith"}, {firstName:"David", lastName:"Jones"}];     
```

To filter a JSON array (and return a new one):
```js
var filtered = json.filter(function(json){return json.lastName == "Smith"});
// or using arrow function
var filtered = json.filter(json => {json.lastName == "Smith"});
```

To loop through a JSON array and create/edit values:
```js
json.forEach(function(i) {return i.fullname = i.firstName + " " + i.lastName});
// or using arrow function
json.forEach(i => {i.fullname = i.firstName + " " + i.lastName});
```



# JQuery
JQuery is a javascript library that makes it easier to change a HTML document. The main way of selecting a tag/id/class.. with JQuery is to use the **$** function with the relevant CSS selector - for example the code below would select all HTML tags whose id="mapid" :
```html
<div id="mapid"></div>
```
```js
$("#mapid");
```

Insert/append content using JQuery. 
```js
$("#mapid").append("Add text");  
$("#mapid").append("<div><a href="google.com">Add link</a><div>");
```
Insert class or style using JQuery. 
```js
$("#mapid").addClass("ssss");
$("#mapid").css("color", "red")
```

Nb. JQuery is simpler way of selecting elements compared to using raw javascript such as *document.getElementById(id)* 


## A Datatable in JS
The **DataTables** library depends on JQuery so that it can be interactive (sortable/searchable/pagination..). 

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
