# d3-StackedTimeline

A timeline built using d3. 

![alt tag](https://github.com/nickcam/d3-StackedTimeline/blob/master/exampleimage/stackedTimeline.PNG)

Couple of examples in the link below, exactly the same as index.html that's committed. 

http://stackedtimeline.azurewebsites.net/

- Rows can have mutiple events which will stack on top of top of each other, depending on how they're defined in the data.
- Use icons in the row label section.
- Use some basic and configurable css for styling.
- Zoomable and pannable, but y axis will retain it's zoom level.
- Configure a bunch of different things in the constructor or by setting chainable properties after instantiation (see below or the code).

Check out the code to see how to use it, or there's a bit of description below.

To create:
```
var options = {
            data: basicData
        }
        
var timeline = d3.select("#timeline")
                        .stackedTimeline(options);
```

All options are optional - defaults will be used if not set in constructor (but data must be set to see anything). 
All options also have getter and setter functions that can be called on the object after instantiation. Setting a value instantly updates the chart and returns the object so chaining is possible.

for example:
```   
var width = timeline.width();
    timeline.width(200).displayCurrent(false).maxDate(new Date());
```

All of the possible options and getter/setters:
```
width (number | default: 1000): The total width of the chart area, including labels.
height (number | default: 300): The total height of the chart area, including labels and time ranges.
labelWidth (number | default: 130): The width of the y axis labels section. This is included in the width setting. So chart area width will be (width - labelWidth).
rowHeight (number | default: 30): The height of each row (ie: y-axis object).
xAxisPosition (string | default: 'bottom'): One of the following strings, 'bottom', 'top', 'both'. Indicates where the date axis will display.
dateAxisTickFormat (d3.time.format.multi | default: The default d3.time.format.multi): The tick format to apply to the date axis.
displayTimeRange (bool | default: true): Whether to display the time range bounds at the start and end of the date axis.
displayScrollbar (bool | default: true) Whether to display a scrollbar at the edges of the chart indicating that there are more records that are hidden.
timeRangeFormat (d3.time.format | default: d3.time.format("%a %d %b %Y")): The time format to format the time range strings.
displayCurrent (bool | default: true): Whether to display the current time as a line on the chart.
currentDateUTC: (bool | default : false): If the current date should display as UTC
updateCurrentMs (number | default: 6000): The amount of milliseconds to update the current line if it's displayed.
limitDatesToData (bool | default: false): Whether to limit panning of the x-axis to the calculated min and max dates specified in the data associated with the chart.
minDate (date | default: undefined): The minimum date the date axis can be panned to. If limitDatesToData is set to true this value is ignored.
maxDate (date | default: undefined): The maximum date the date axis can be panned to. If limitDatesToData is set to true this value is ignored.
transition (bool | default: true): Whether to transition the display of the data. Will transition the width of each rect from 0 to it's width. Transtion will only occur when changing the underlying data, ie: so on initila load or pasing a value to data() setter.
transitionDuration (number | default: 1000): The millisecond duration of the transition.
data (object | default: undefined): The data to apply to the chart. Format described below.
```

'data' of the chart is an object with two array properties: config and rows:
- config objects can contain 3 properties - className, height & borderRadius.
- The order in which config objects are defined in the array is important and relates to the row objects events array.
- The data example will set a className on all events at index 0 to be 'a-class' with no border radius set. All events at index 1 will have a className of 'b-class' and a border radius of 10.

rows defines the actual data. 

Each row entry relates to a y-axis entry. 
- 'label': sets the tick label text.
- 'labelIcon': sets a unicode value so you can use an icon font to display an icon. Set the font family using css (text.label-icon).
- 'labelIconClassName': can be set to add extra styles or override existing ones for the label icons on a per row basis.
- 'events': This is an array that must contain a 'start' and 'end' property which are both dates.
  - Optionally it can include 'hoverText' to display in a popup when the event is hovered over.
  - Can also have a 'className' property to override the className set on it in the config object.

data Example: (obviously needs real dates for start and end).
```
{
  config: [{ className: "a-class", height: "15" }, { className: "b-class", borderRadius: 10, height: "25" }],
  rows: [
  {
    label: "Task ABC",
    labelIcon: "\uf0ae",
    events: [{ hoverText: "The first event", start: new Date(), end: new Date() },
             { hoverText: "Second level event", start: new Date(), end: new Date() }]
            },
  {
    label: "Person 123",
    labelIcon: "\uf007",
    events: [{ hoverText: "first level", start: new Date(), end: new Date() },
             { className: "replace-b-class", start: new Date(), end: new Date() }]
  }]
}
```

    
