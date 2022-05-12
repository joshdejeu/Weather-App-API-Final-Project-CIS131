<template>
    <div id="phone_container">
        <div id="weather_dispaly">
            <div id="camera_lip"></div>
            <div id="weather_header">
                <div id="weather_header1"></div>
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
                    <img id="body_image" src="../assets/cloud1.png" alt="">
                </div>

                <div id="body_temp">{{temp}}<div id="degree"></div></div>
                <div id="body_weathertype">{{type}}</div>
                <div id="body_date">{{date}}</div>

                <div id="body_multidata">
                    <div class="multidata">
                        <img src="" alt="">
                        <h3 class="multidata_title">{{wind}} km/h</h3>
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
import axios from 'axios';

export default{
    name: 'PhoneContainer',
    components: {
    UserInput
    },
    props:{
    },
    data(){
        return{
            city: "Minsk",
            temp: 21,
            type: "Thunderstorm",
            date: "Tuesday, May 5th",
            wind: 0,
            humidity: 0,
            pressure: 0,
            keyAPI: "b14a0f6f86b259ba7073b364662a0387",
            zipcode: 65616,
            countrycode: "US",
            unit: "imperial",
        }
    },
    mounted() {
        // window.addEventListener("resize", this.myEventHandler);
        this.error_status = 
        this.callAPI();
    },
    methods: {
        emit_end(zip){
            this.zipcode = zip;
            this.countrycode = "US";
            // Now the zipcode has been updated so we call the  API
            this.callAPI();
        },
        callAPI(){
            // Here we call the API to get data from their servers with updated ZIP
            var link = `https://api.openweathermap.org/data/2.5/weather?zip=${this.zipcode},${this.countrycode}&appid=${this.keyAPI}&units=${this.unit}`
            axios.get(link)
            .then(response => {
                // JSON responses are automatically parsed.
                console.log("Success\n",response);
                this.city= response.data.name;
                this.temp= response.data.main.temp.toFixed(0);
                this.wind= response.data.wind.speed;
                this.humidity= response.data.main.humidity;
                this.pressure= response.data.main.pressure;
                this.type= response.data.weather[0].description;
                this.$refs.userInput.$el.className = "true";
            })
            .catch(e => {
                this.$refs.userInput.$el.className = "false";
                console.log("Error\n",e);
            })
            console.log("First page",this.$refs.userInput.$el.className);

        }
    },
}
</script>

<style scoped>
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
    font-size: 2.8vw;
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
    height: 35%;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;    
    /* border: 1px solid lime; */
}
#body_image{
    height: 100%;
    transform: scale(0.75);
    display: block;
    /* border: 1px solid red; */
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
    font-size: 25vw;
    /* border: 1px solid lime; */
    width: 100%;
    height: 35%;
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
    font-size: 5vw;
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    color: white;
    text-shadow: 0px 0px 50px rgba(0, 0, 0, 0.192);
}
#body_date{
    height: 8%;
    font-size: 2.5vw;
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    color: rgba(255,255,255,0.6);
    text-shadow: 0px 0px 50px rgba(0, 0, 0, 0.142);
}
#body_multidata{
    height: 15%;
    width: 100%;
    display: flex;
    justify-content: space-evenly;
    /* border: 1px solid lime; */
    text-shadow: 0px 0px 50px rgba(0, 0, 0, 0.192);
}
.multidata_title{
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    color: white;
    text-align: center;
    font-size: 2.8vw;
}
.multidata_info{
    font-size: 2.2vw;
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    color: rgba(255,255,255,0.6);
    text-align: center;
}
#body_multidata>div{
    /* border: 1px solid red; */
}
#phone_container{
    position: relative;
    width: 20%;
    aspect-ratio: 0.50;
    border-radius: 20%/10%;
    border: 10px solid black;
    overflow: visible;
    background: #000918;
    box-shadow: 0px 0px 100px rgba(255, 255, 255, 0.3);
}
#weather_dispaly{
    position: relative;
    widows: 100%;
    height: 75%;
    border-radius: 19%/11.7%;

    background: linear-gradient(15deg, #1070f7 , #12d0ff);
    box-shadow:
     inset 0px 0px 5px 1px rgba(99, 219, 255,0.7),
     0px -10px 10px rgba(27, 171, 255, 0.3),
     10px -0px 10px rgba(27, 171, 255, 0.3),
     -10px -0px 10px rgba(27, 171, 255, 0.3);
    animation: backgroundMove 7s linear infinite;
    background-size: 200% 200%;
}
@keyframes backgroundMove {
    0%{background-position: 100% 100%;}
    20%{background-position: 100% 0%;}
    50%{background-position: 0% 0%;}
    100%{background-position: 100% 100%;}
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
 }
#weather_header>div{
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}
#weather_header2{
    position: relative;
}
#header_city{
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-weight: 600;
    font-size: 1.5em;
}
#header_img{
    position: relative;
    width: 20%;
    min-width:20%;
    filter: brightness(0%) invert(100%);
}
#weather_header3{
    flex-direction: column;
    cursor: pointer;
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
        width: 25%;
    }
}
@media only screen and (max-width: 1300px){
    #phone_container{
        width: 30%;
    }
}
@media only screen and (max-width: 1100px){
    #phone_container{
        width: 40%;
    }
}
@media only screen and (max-width: 900px){
    #phone_container{
        width: 50%;
    }
}
@media only screen and (max-width: 800px){
    #phone_container{
        width: 60%;
    }
}
@media only screen and (max-width: 700px){
    #phone_container{
        width: 65%;
    }
}
</style>
