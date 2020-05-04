# Riyadh COVID 19 DASHBOARD CASE STUDY
* In this project we are going to create a simple dashboard to track COVID 19 disease in Riyadh Area
* Download the project data from the following URL: https://github.com/mohammed16/Riyadh_Covid19_Dashboard
* Open Your IDE for example Visual Studio Code and create the basic HTML page
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1">
    <title> COVID 19 Dashboard </title>
</head>
<body>

</body>
</html>

```
* in the head section add all the following bootstrap and jquery libraries
```html
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"></script>

```
* in body section add the following elements to create a navbar
```html
<nav class="navbar navbar-dark bg-dark static-top">
    <div class="container">
        <a class="navbar-brand" href="#">Riyadh COVID 19 Dashboard</a>
        <form class="form-inline" action="#">
            <input class="form-control mr-sm-2" type="text" id="searchText" 
		placeholder="Search">
            <button class="btn btn-primary" type="button">Search</button>
        </form>
    </div>
</nav>
```

* Under nav element add the following div tag to create the map container element
```html
<div class="container-fluid" id="mapContainer"></div>
```
* after map container element add the following code to create footer for your web page
```html
<footer class="footer">
    <div class="container">
        <div class="row">
            <div class="col-lg-6 h-100 text-center text-lg-left my-auto">
                <p class="text-muted small mb-4 mb-lg-0">
                    &copy; Geocode 2020. All Rights Reserved.
                </p>
            </div>
        </div>
    </div>
</footer>

```
* create a new folder, for folder name use 'css' and inside css folder create a new file and use 'style.css' for file name

![Alt text](/img/add_css.gif)

* write the following code inside style.css file
```css
.container-fluid{
    padding: 0rem;
    height: 83vh;
}
footer.footer {
    padding-top: 1rem;
    padding-bottom: 1rem;
}

```
* write the following statement in the head section to include the style.css in your web page
```html
<link rel="stylesheet" href="css/style.css">
```
* write the following statements in head section to include HERE MAPs API
```html
<script src="https://js.api.here.com/v3/3.1/mapsjs-core.js"></script>
<script src="https://js.api.here.com/v3/3.1/mapsjs-service.js"></script>
<script src="https://js.api.here.com/v3/3.1/mapsjs-ui.js"></script>
<link rel="stylesheet" href="https://js.api.here.com/v3/3.1/mapsjs-ui.css" />
<script src="https://js.api.here.com/v3/3.1/mapsjs-mapevents.js"></script>

```
* Create a new folder, and use 'js' for folder name and inside js folder create a new file and use 'script.js' for file name

![Alt text](/img/add_js.gif)

* Write the following statements in script.js file
```javascript
var platform = new H.service.Platform({
    'apikey':'YOUR API KEY'
});
var layers = platform.createDefaultLayers();
var map = new H.Map(
    document.getElementById('mapContainer'),
    layers.vector.normal.map,
    {
        zoom: 12,
        center:{lat:24.693736, lng:46.678896}
    }
);
var ui = new H.ui.UI.createDefault(map,layers);
var mapEvents = new H.mapevents.MapEvents(map);
var behavior = new H.mapevents.Behavior(mapEvents);

```
* in index.html page at the end of the body section add the following statement to include the script.js file in your web page
```html
<script src="js/script.js"></script>
```

* in Visual Studio code, go to extension and search for Live Server Extention and install it then, then click on Go Live icon

![Alt text](/img/go_live.gif)

## Add Districts data to your web page

* in head section write the following script to include mapjs-data module
```html
<script src="https://js.api.here.com/v3/3.1/mapsjs-data.js"></script>
```
* open script.js and add the following statements to create styles for districts
```javascript
var lest50 = {fillColor: 'rgba(254, 229, 217, 0.7)', lineWidth: 0};
var lest100 = {fillColor: 'rgba(252, 174, 145, 0.7)', lineWidth: 0};
var lest150 = {fillColor: 'rgba(251, 106, 74, 0.7)', lineWidth: 0};
var lest250 = {fillColor: 'rgba(222, 45, 38, 0.7)', lineWidth: 0};
var lest400 = {fillColor: 'rgba(165, 15, 21, 0.7)', lineWidth: 0};

```

* write the following code to add districts.geojson layer
```javascript
var districts = new H.data.geojson.Reader('data/districts.geojson',{
    disableLegacyMode: true,
    style: function(mapObject){
        if(mapObject.getData().ACTIVE <= 50){
            mapObject.setStyle(lest50);
        }else if(mapObject.getData().ACTIVE <= 100){
            mapObject.setStyle(lest100);
        }else if(mapObject.getData().ACTIVE <= 150){
            mapObject.setStyle(lest150);
        }else if(mapObject.getData().ACTIVE <= 250){
            mapObject.setStyle(lest250);
        }else if(mapObject.getData().ACTIVE <= 400){
            mapObject.setStyle(lest400);
        }
    }
});
districts.parse();
map.addLayer(districts.getLayer());

```
* write the following code to show infobubble when users click on district
```javascript
districts.getLayer().getProvider().addEventListener('tap', (evt) => {
    var coords = map.screenToGeo(
        evt.currentPointer.viewportX,
        evt.currentPointer.viewportY
    );
    var postion = {lat: coords.lat , lng: coords.lng};
    var dat = evt.target.getData();
    var bubble =  new H.ui.InfoBubble(postion, {
        content: '<b>NAME:</b><br>' + dat['NAME'] + '<br>' +
            '<b>CONFIRMED:</b><br>' + dat['CONFIRMED'] + '<br>' +
            '<b>RECOVERED:</b><br>' + dat['RECOVERED'] + '<br>'+ 
            '<b>DEATH:</b><br>' + dat['DEATH'] + '<br>'+ 
            '<b>ACTIVE:</b><br>' + dat['ACTIVE'] 
    });
    ui.addBubble(bubble);
},false);

```

## Add Hospitals data to your web page

* Write the following code to add Hospitals data
```javascript
// Hospital Icons
var markerIcon12 = new H.map.Icon('img/hosp.png', {size: {w: 15, h: 15}});
var markerIcon15 = new H.map.Icon('img/hosp.png', {size: {w: 15, h: 15}});
var markerIcon30 = new H.map.Icon('img/hosp.png', {size: {w: 25, h: 25}});
var markerIcon41 = new H.map.Icon('img/hosp.png', {size: {w: 35, h: 35}});
var markerIcon71 = new H.map.Icon('img/hosp.png', {size: {w: 50, h: 50}});

// create hospital layer
var hospitals = new H.data.geojson.Reader('data/hospitals.geojson',{
    disableLegacyMode: true,
    style: function(mapPoint){
        if(mapPoint.getData().CAPACITY == "12%") {
            mapPoint.setIcon(markerIcon12);
        }else if(mapPoint.getData().CAPACITY == "15%"){
            mapPoint.setIcon(markerIcon15);
        }else if(mapPoint.getData().CAPACITY == "30%"){
            mapPoint.setIcon(markerIcon30);
        }else if(mapPoint.getData().CAPACITY == "41%"){
            mapPoint.setIcon(markerIcon41);
        }else if(mapPoint.getData().CAPACITY == "71%"){
            mapPoint.setIcon(markerIcon71);
        }
    }
});
hospitals.parse();
map.addLayer(hospitals.getLayer());

// add InfoBubble on Marker click event
hospitals.getLayer().getProvider().addEventListener('tap', (evt) => {
    var dat = evt.target.getData();
    var bubble =  new H.ui.InfoBubble(evt.target.getGeometry(), {
        content: '<b>Hospital Name:</b><br>' + dat['NAME'] + '<br>' +
            '<b>Phone Number:</b><br>' + dat['PHONE_NO'] + '<br>' +
            '<b>Capacity:</b><br>' + dat['CAPACITY'] + '<br>'+ 
            '<b>Patients:</b><br>' + dat['PATIENTS'] 
    });
    ui.addBubble(bubble);
},false);

```

## Add Patients data to your web page
* Write the following code to add patients data
```javascript
// Patients Icons
var active = new H.map.Icon('img/active.png',{size:{w: 15, h:15}});
var death = new H.map.Icon('img/death.png',{size:{w: 15, h:15}});
var recovered = new H.map.Icon('img/recovered.png',{size:{w: 15, h:15}});
// create patients layer
var patients = new H.data.geojson.Reader('data/patients.geojson',{
    disableLegacyMode: true,
    style: function(mapPoint){
        if(mapPoint.getData().STATUS == "ACTIVE") {
            mapPoint.setIcon(active);
        }else if(mapPoint.getData().STATUS == "DEATH"){
            mapPoint.setIcon(death);
        }else if(mapPoint.getData().STATUS == "RECOVERED"){
            mapPoint.setIcon(recovered);
        }
    }
});
patients.parse();
map.addLayer(patients.getLayer());
// add InfoBubble on Marker click event
patients.getLayer().getProvider().addEventListener('tap', (evt) => {
    var dat = evt.target.getData();
    var bubble =  new H.ui.InfoBubble(evt.target.getGeometry(), {
        content: '<b>Building:</b><br>' + dat['BUILDING'] + '<br>' +
            '<b>ADDRESS:</b><br>' + dat['ADDRESS'] + '<br>' +
            '<b>STATUS:</b><br>' + dat['STATUS']
    });
    ui.addBubble(bubble);
},false);

```

## Show Statistics

* In index.html page and after map container div add the following elements

```html
<div class="stats">
    <div class="card-box bg-blue">
        <div class="inner">
            <h3 id="confirmed"></h3><p>CONFIRMED</p>
        </div>
    </div>
    <div class="card-box bg-orange">
        <div class="inner">
            <h3 id="active"></h3><p>ACTIVE</p>
        </div>
    </div>
    <div class="card-box bg-green">
        <div class="inner">
            <h3 id="recovered"></h3><p>RECOVERED</p>
        </div>
    </div>
    <div class="card-box bg-red">
        <div class="inner">
            <h3 id="death"></h3><p>DEATH</p>
        </div>
    </div>
</div>

```

* In style.css add the following css statements
```css
.stats{
    position: absolute;
    top: 75px;
    left: 20px;
    width: 30vh;
}
.stats .card-box{
    box-shadow: darkgray 4px 4px 4px;
    background-color: white;
    padding: 15px;
    margin-bottom: 10px;
}
.bg-blue {
    background-color: #00c0ef !important;
}
.bg-green {
    background-color: #00a65a !important;
}
.bg-orange {
    background-color: #f39c12 !important;
}
.bg-red {
    background-color: #d9534f !important;
}

```
* In script.js type the following code to calculate the statistics 
```javascript
var sumActive = 0, sumRecovered = 0, sumDeath = 0, sumConfirmed = 0;
fetch('data/districts.geojson')
    .then(res => res.json())
    .then(res => {
        res.features.forEach(function(feature){
            sumActive += feature.properties.ACTIVE ;
            sumRecovered += feature.properties.RECOVERED;
            sumDeath += feature.properties.DEATH;
            sumConfirmed += feature.properties.CONFIRMED;
        });
        document.getElementById('active').innerHTML = sumActive;
        document.getElementById('death').innerHTML = sumDeath;
        document.getElementById('recovered').innerHTML = sumRecovered;
        document.getElementById('confirmed').innerHTML = sumConfirmed;
    });

```
* Save changes and open your web page in your web browser.
![Alt text](/img/screenshot1.png)
