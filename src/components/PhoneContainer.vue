<template>
    <div id="phone_container">
        <div id="weather_dispaly">
            <div id="camera_lip"></div>
            <div id="weather_header">
                <div id="weather_header1">
                    <div class="latlong">{{coords}}</div>
                    <!-- <div class="latlong">Lon: {{lon}}</div> -->
                </div>
                <div id="weather_header2">
                    <img src="../assets/pin.png" id="header_img">
                    <h2 id="header_city">{{city}}</h2>
                </div>
                <div id="weather_header3">
                    <div></div>
                    <div></div>
                    <div></div>
                </div>
            </div>
            <div id="weather_body">

                <div id="body_image_container">
                    <img id="body_image" :src="icon" alt="">
                </div>

                <div id="body_temp">{{temp}}<div id="degree"></div></div>
                <div id="body_weathertype">{{type}}</div>
                <div id="body_date">{{date}}</div>

                <div id="body_multidata">
                    <div class="multidata">
                        <img src="" alt="">
                        <h3 class="multidata_title">{{wind}} {{wind_measure}}</h3>
                        <h3 class="multidata_info">Wind</h3>
                    </div>
                    <div class="multidata">
                        <img src="" alt="">
                        <h3 class="multidata_title">{{humidity}}%</h3>
                        <h3 class="multidata_info">Humidity</h3>
                    </div>
                    <div class="multidata">
                        <img src="" alt="">
                        <h3 class="multidata_title">{{pressure}} Pa</h3>
                        <h3 class="multidata_info">Pressure</h3>
                    </div>
                </div>
            </div>
        </div>
        <div id="notification_container">
            <div id="notification">
                <img src="../assets/cloud.png" id="notification_logo">
                <div id="notification_text">test</div>
            </div>
        </div>
        <UserInput @emit_start="emit_end" ref="userInput"/>
    </div>
</template>

<script>
import UserInput from "./UserInput"
export default{
    name: 'PhoneContainer',
    components: {
    UserInput
    },
    props:{
    },
    data(){
        return{
            lat: "123.45",
            lon: "67.89",
            city: "Loading",
            temp: 0,
            type: "cloudy",
            date: "Tuesday, May 5th",
            wind: 0,
            humidity: 0,
            pressure: 0,
            wind_measure: "mi/h",
            coords: "00°N 00°W",
            icon: "../assets/icons/josh_sun.png",
        }
    },
    methods: {
        // A.0) Converting data received from API to data to be displayed
        emit_end(city,temp,wind,humidity,pressure,type,lat,lon,unit,ico){
            this.city=city;
            this.temp=temp;
            this.wind=wind;
            this.humidity=humidity;
            this.pressure=pressure;
            this.type=type;
            this.icon=`../assets/icons/${this.setIcon(ico)}.png`;
            // A.1) I want to format lat and long into a single coordinate (Exmaple: "42°N 64°W")
            // A.2) here we check North, East, West, and South and make a string to display
            // A.3) parse the variable to an int, if it is positive for Lattitude it is North, for Longitude it is East, and the opposite for negatives
            if(parseInt(lat)>0){this.lat=lat+"°N";}
            else{this.lat=Math.abs(lat)+"°S";}
            if(parseInt(lon)>0){this.lat=lat+"°E";}
            else{this.lon=Math.abs(lon)+"°W";}
            this.coords = this.lat+" "+this.lon;
            if(unit=="metric"){this.wind_measure="m/s";}
            else{this.wind_measure="mi/h";}
        },
        // B.0) Set the icon image based on API weather type
        setIcon(icon){
            // B.1) These are the most common icons I could find so I created images to display when we see this in the API response
            if(icon=="01n"){return "josh_sun";}
            if(icon=="02n"){return "josh_cloud_scat";}
            if(icon=="03n"){return "josh_cloud_scat";}
            if(icon=="04n"){return "josh_cloud3";}
            if(icon=="50d" || icon=="50n"){return "josh_fog";}
            if(icon=="10n"){return "josh_rain_heavy";}
            // B.2) since not all icons are accounted for, the default icon is set here
            else{return "josh_sun";}
        }
    },
}
</script>

<style scoped>
#weather_dispaly{
    position: relative;
    widows: 100%;
    height: 75%;
    border-radius: 19%/11.7%;
    background: linear-gradient(15deg, #1070f7,#12d0ff);
    /* background: linear-gradient(15deg, #1070f7, transparent, #12d0ff), url('../assets/snow.png'); */
    /* background: radial-gradient(rgba(0,0,0,0.2), rgba(0,0,0,0.4)), url('../assets/raincloud.png'); */
    /* background: radial-gradient(rgba(0,0,0,0.2), rgba(0,0,0,0.4)), url('../assets/snow.png'); */
    box-shadow:
     inset 0px 0px 5px 1px rgba(99, 219, 255,0.7),
     0px -10px 10px rgba(27, 171, 255, 0.3),
     10px -0px 10px rgba(27, 171, 255, 0.3),
     -10px -0px 10px rgba(27, 171, 255, 0.3);
    animation: backgroundMove 7s linear infinite;
    /* animation: snow 7s linear infinite; */
    background-size: 200% 200%;
    /* so Psuedo ::after appears under */
    transform-style: preserve-3d;
}
@keyframes backgroundMove {
    0%{background-position: 100% 100%;}
    20%{background-position: 100% 0%;}
    50%{background-position: 0% 0%;}
    100%{background-position: 100% 100%;}
}
@keyframes snow {
    0%{background-position: 0% 100%;}
    100%{background-position: 100% 0%;}
}
#phone_container{
    transition: 0.5s ease;
    position: relative;
    /* width: 30%; */
    height: 80vh;
    aspect-ratio: 0.50;
    border-radius: 20%/10%;
    border: 10px solid black;
    overflow: visible;
    /* background: #000918; */
    background: #051838;
    box-shadow:
    0px 0px 100px rgba(255, 255, 255, 0.3),
    inset 0px 0px 10px rgba(9, 47, 112, 0.5);
}
#notification_container{
    position: absolute;
    top: 5%;
    left: 0;
    width: 100%;
    height: 8%;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: 0.5s ease;
    opacity: 0;
    margin-top: -60px;
}
#notification_text{
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    font-size: 15px;
    text-shadow: 1px 1px 2px rgba(61, 61, 61, 0.5);
}
#notification_logo{
    height: 50%;
    background-color: rgba(255, 255, 255, 0.808);
    border-radius: 10%;
    position: relative;
    margin-left: 5%;
    margin-right: 5%;
}
#notification{
    position: relative;
    width: 80%;
    height: 100%;
    border-radius: 7% / 32%;
    /* backdrop-filter: blur(5px); */
    z-index: 3;
    display: flex;
    justify-content: left;
    align-items: center;

}
#weather_body{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 86.5%;
}
#body_image_container{
    width: 100%;
    height: 45%;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;    
}
#body_image{
    height: 100%;
    transform: scale(0.75);
    display: block;
    -webkit-filter: drop-shadow(10px 10px 30px rgba(0, 0, 0, 0.35));
    filter: drop-shadow(10px 10px 30px rgba(0, 0, 0, 0.35));
}
#degree{
    width: 7px;
    height: 7px;
    border-radius: 50%;
    box-shadow: 0px 0px 0px 3px rgb(193, 218, 231);
}
#body_temp{
    color: white;
    font-family: 'Roboto Condensed', serif;
    font-weight: bold;
    font-size: 140px;
    width: 100%;
    height: 25%;
    margin: 0;
    padding: 0;
    text-align: center;
    line-height: 75%;
    display: flex;
    justify-content: center;
    align-items: start;
    text-shadow: 0px 0px 50px rgba(0, 0, 0, 0.172);
}
#body_weathertype{
    height: 7%;
    font-size: 25px;
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    color: white;
    text-shadow: 0px 0px 50px rgba(0, 0, 0, 0.192);
}
#body_date{
    height: 3%;
    font-size: 12px;
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    color: rgba(255,255,255,0.6);
    text-shadow: 0px 0px 50px rgba(0, 0, 0, 0.142);
}
#body_multidata{
    height: 20%;
    width: 100%;
    position: relative;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    text-shadow: 0px 0px 50px rgba(0, 0, 0, 0.192);
}
#body_multidata::after{
    content: '';
    position: absolute;
    top: 20%;
    left: 15%;
    width: 70%;
    border-radius: 5px;
    height: 2px;
    background-color: rgba(255,255,255,0.15);
}
.multidata_title{
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    color: white;
    text-align: center;
    font-size: 15px;
}
.multidata_info{
    font-size: 12px;
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    color: rgba(255,255,255,0.6);
    text-align: center;
}
#weather_dispaly::after{
    content: '';
    transform: translateZ(-1px);
    position: absolute;
    bottom: -2%;
    left: 17.5%;
    width: 65%;
    height: 10%;
    background: rgba(16, 155, 247, 0.3);
    box-shadow: 
    0px 0px 15px rgba(16, 155, 247, 0.85),
    inset 0px 0px 15px rgba(85, 178, 240, 0.7);
    border-radius: 20px;
}
/* #weather_display:before{
    content: "test";
    position: absolute;
    height: 700px;
    width: 500px;
    top: 0;
    left: 0;
    background: linear-gradient(15deg, rgb(29, 231, 29), rgb(231, 154, 103));
    z-index: 3;
}
 */
#camera_lip{
    z-index: 4;
    width: 40%;
    height: 5%;
    position: absolute;
    top: 0;
    left: 30%;
    background: black;
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 20px;
    box-shadow: inset 0px -10px 15px rgba(27, 171, 255, 0.35);
}
 #weather_header{
     width: 100%;
     height: 13%;
     display: flex;
     justify-content: space-evenly;
     align-items: end;
     position: relative;
 }
 #status_color{
     width: 5px;
     height: 5px;
     background-color: red;
     border-radius: 50%;
     margin-right: 5px;
 }
 .latlong{
    border-radius: 50px;
    border: 1px solid rgba(255, 255, 255, 0.7);
    padding: 2px 8px;
 }
#weather_header1{
    position: relative;
    width: 40%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: start;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-weight: 400;
    font-size: 0.8em;
    padding-left: 10%;
}
#weather_header2{
    width: 100%;
    position: relative;
    display: flex;
    justify-content: start;
    align-items: center;
}
#weather_header3{
    position: absolute;
    right: 0;
    flex-direction: column;
    cursor: pointer;
    width: 10%;
    display: flex;
    justify-content: center;
    align-items: start;
}
#header_city{
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-weight: 600;
    font-size: 1.4em;
}
#header_img{
    position: relative;
    min-width: 10%;
    max-width: 10%;
    filter: brightness(0%) invert(100%);
}
#weather_header3>div{
    width: 4px;
    height: 4px;
    background: white;
    margin: 2px;
    border-radius: 50%;
}

@media only screen and (max-width: 1500px){
    #phone_container{
        /* width: 25%; */
        /* transform: scale(0.9); */
    }
}
@media only screen and (max-width: 1300px){
    #phone_container{
        /* transform: scale(0.8); */
    }
}
@media only screen and (max-width: 1100px){
    #phone_container{
        /* transform: scale(0.7); */
    }
}
@media only screen and (max-width: 900px){
    #phone_container{
        /* transform: scale(0.6); */
    }
}
@media only screen and (max-width: 800px){
    #phone_container{
        /* transform: scale(0.5); */
    }
}
@media only screen and (max-width: 700px){
    #phone_container{
        /* transform: scale(0.4); */
    }
}
</style>
