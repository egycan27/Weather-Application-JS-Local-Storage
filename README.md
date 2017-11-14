# Weather-Application-JS-Local-Storage
Using Rest API, Local Storage and JS for weather pairing 
First go to https://openweathermap.org/ to create an api for your appilication 
Create an appilication using your favourite editor have, I used PHP storm. \
As your app needs to have the following structure : Weather Folder / CSS contains all the styling sheets , imgs contain all the images of the app , JS conatins the logic of your app 
Two other HTML files Index, Weather 
declare the following var APPID = Your ID optatained from the weather rest api 
var temp;
var loc;
var icon;
var humidity;
var wind;
var direction;
var fill, first,enteredFormName,TestVar,city,winds,Maximum_Temprature,Minimum_Temprature,Visibility,cloud ;
Create a function called update weather : function update(weather) {
    icon.src = "imgs/codes/" + weather.code + ".png"
    humidity.innerHTML = weather.humidity;
    wind.innerHtml = weather.wind;
    direction.innerHTML = weather.direction;
    loc.innerHTML = weather.location;
    temp.innerHTML = weather.temp;
    fill.innerHTML = weather.location;
    winds.innerHTML = weather.wind+" mph";
    Maximum_Temprature.innerHTML = weather.max_temp+ " C";
    Minimum_Temprature.innerHTML = weather.min_temp+ "C";
    Visibility.innerHTML = weather.visibility+ "KM";
    cloud.innerHTML = weather.cloud;
   }

}
window.onload = function () {
    temp = document.getElementById("temperature");
    loc = document.getElementById("location");
    icon = document.getElementById("icon");
    humidity = document.getElementById("humidity");
    wind = document.getElementById("wind");
    direction = document.getElementById("direction");
    fill = document.getElementById("fill");
    winds = document.getElementById("winds");
    Maximum_Temprature = document.getElementById("Maximum_Temprature");
    Minimum_Temprature = document.getElementById("Minimum_Temprature");
    Visibility = document.getElementById("Visibility");
    cloud = document.getElementById("cloud");


    /* NEW */
   /* if(navigator.geolocation){
var showPosition = function(position){
	    updateByGeo(position.coords.latitude, position.coords.longitude);
	}
	navigator.geolocation.getCurrentPosition(showPosition);
    } else {*/

        //updateByZip(87110);
    updateByCity(localStorage.getItem("item"));
    //}

}

/* NEW */

function updateByGeo(lat, lon){
    var url = "http://api.openweathermap.org/data/2.5/weather?" +
	"lat=" + lat +
	"&lon=" + lon +
	"&APPID=" + APPID;
    sendRequest(url);    
}


function updateByZip(zip){
    var url = "http://api.openweathermap.org/data/2.5/weather?" +
	"zip=" + zip +
	"&APPID=" + APPID;
    sendRequest(url);
}

function updateByCity(city){
    var url = "http://api.openweathermap.org/data/2.5/weather?" +
        "q=" + city +
        "&APPID=" + APPID;
    sendRequest(url);
}
function sendRequest(url){
    var xmlhttp = new XMLHttpRequest();
    xmlhttp.onreadystatechange = function() {
	if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
            var data = JSON.parse(xmlhttp.responseText);
	    var weather = {};
	    weather.code = data.weather[0].id;
        weather.main = data.weather[0].main;
        weather.description = data.weather[0].description;
        weather.max_temp = K2C(data.main.temp_max);
        weather.min_temp = K2C(data.main.temp_min);
        weather.visibility = data.visibility;
	    weather.humidity = data.main.humidity;
	    weather.cloud = data.clouds.all;
        weather.wind = data.wind.speed;
        weather.wind = data.wind.speed;
        weather.wind = data.wind.speed;
        weather.wind = data.wind.speed;
        weather.wind = data.wind.speed;
        weather.pressure = data.main.pressure;
	    /* NEW */
	    weather.direction = degreesToDirection(data.wind.deg)
	    weather.location = data.name;
	    /* NEW */
	    weather.temp = K2C(data.main.temp);
	    update(weather);
	}
    };

    xmlhttp.open("GET", url, true);
    xmlhttp.send();    
}

function degreesToDirection(degrees){
    var range = 360/16;
    var low = 360 - range/2;
    var high = (low + range) % 360;
    var angles = ["N", "NNE", "NE", "ENE", "E", "ESE", "SE", "SSE", "S", "SSW", "SW", "WSW", "W", "WNW", "NW", "NNW"];
    for( i in angles ) {
	if(degrees >= low && degrees < high){
	    console.log(angles[i]);
	    return angles[i];
	}
	low = (low + range) % 360;
	high = (high + range) % 360;
    }
    return "N";
    
}

function K2F(k){
    return Math.round(k*(9/5)-459.67);
}

function K2C(k){
    return Math.round(k - 273.15);
}


