<template>
<div id="user_container">
    <div id="textinput_container">
        <div id="img_text">
            <input id="text_input" type="text" placeholder="Enter ZIP">
            <img src="../assets/pin.png" id="zip_image" @click="logZip($event)"/>
        </div>
    </div>
    
    <div id="btn_container">
        <input class="btn" id="btn_c" type="button" value="C°" @click="changeTempUnit('C')">
        <input class="btn" id="btn_f" type="button" value="F°" @click="changeTempUnit('F')">
    </div>
    <div id="home_bar"></div>
</div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'UserInput',
    components: {

    },
    data() {
    return {
        lastWorkingZip: 10101,
        status: "",
        key: "",
        zipcode: 11111,
        date: "Tuesday, May 5th",
        keyAPI: "b14a0f6f86b259ba7073b364662a0387",
        countrycode: "US",
        unit: "imperial",
        APIerror: 'none',
    };
    },
    mounted() {
        this.key = "random";
        this.callAPI(this.setRandomZip(), this.key);
    },
    methods:{
        // Create random zipcode on load
        setRandomZip(){
            // generate random num from 1 to 99950 (the range valid US zipcodes)
            var generatedZip = Math.floor(Math.random() * (99950) + 1);
            // if the generated number length is less tahn 5 we have to add 0's to fill
            // EXAMPLE: 1 is not a valid zip, but 00001 is
            if(generatedZip.toString().length!=5){
                // target length is 5, so remove the length of the generated number
                // this is how many 0's we need to add to the beginning
                // so the number "5" will now be "00005"
                generatedZip = ("0".repeat(5-generatedZip.toString().length)+generatedZip);
            }
            this.zipcode = generatedZip;
            return(this.zipcode);
        },
        callAPI(zip, key, color, text, notification){
            // Here we call the API to get data from their servers with updated ZIP
            var link = `https://api.openweathermap.org/data/2.5/weather?zip=${zip},${this.countrycode}&appid=${this.keyAPI}&units=${this.unit}`
            axios.get(link)
            .then(response => {
                // since this zipcode is successful in the API database, we store it as the most recent working zipcode
                this.lastWorkingZip = this.zipcode;
                // send all weather data to parent component
                this.$emit('emit_start',response.data.name,response.data.main.temp.toFixed(0),response.data.wind.speed,response.data.main.humidity,response.data.main.pressure,response.data.weather[0].description,response.data.coord.lat.toFixed(0),response.data.coord.lon.toFixed(0),this.unit,response.data.weather[0].icon);
                this.status = "ZIP code Successfully Found";
                if(key!="random"||key!="unit"){this.notifySuccess(color,text,notification);}// show notification only if user calls API
            })
            .catch(e=>{
                this.APIerror = e;
                // if API returns error from random zip, try again, else it is a user error and we show that using notifications
                if(key!="random"){
                    // ONLY when use inputs ZIP, show error notification
                    this.status = "API has no data for this ZIP";
                    // Update the zipcode to the last working zipcode, if user changes to C or F so it doesn't send the zipcode that API doesn't have data for and then the screen doesn't update
                    this.zipcode = this.lastWorkingZip;
                    this.notifyError(color,text,notification);
                }
                // since computer generated the ZIP here, the api has no data for it, so we generate a new zip and try again
                else{this.callAPI(this.setRandomZip(), this.key);}
            })
        },
        changeTempUnit(val){
            var btnC = document.getElementById('btn_c');
            var btnF = document.getElementById('btn_f');
            if(val == "F"){
                btnF.style.background="linear-gradient(25deg,rgb(18, 184, 226), rgb(16, 104, 219))";
                btnC.style.background="transparent";
                btnF.style.color="black";
                btnC.style.color="rgb(18, 184, 226)";
                this.unit = "imperial";
            }else{
                btnC.style.background="linear-gradient(-25deg,rgb(18, 184, 226), rgb(16, 104, 219))";
                btnF.style.background="transparent";
                btnC.style.color="black";
                btnF.style.color="rgb(18, 184, 226)";
                this.unit = "metric"
            }
            // updated temp unit now re-call API
            this.callAPI(this.zipcode, "unit")
        },
        // Checks for a valid zip code
        validateZip(zipcode){
            var nonTest = zipcode.length==0;
            var numTest = !isNaN(zipcode);
            var lenTest = zipcode.length==5;
            var ranTest = parseInt(zipcode)>0 && parseInt(zipcode)<99951;
            // .length == 0 returns true if field is empty
            // !isNaN() returns true if is a number
            // .length == 5 returns true if exactly 5 numbers
            // 0>x<99951 because valid US zipcodes are between 00001 and 99950
            // We will check each statemenet individually to update the error message and get useful info to the user
            this.status="ZIP entry can not be empty";
            if(nonTest){return;}
            this.status="ZIP must be all numbers";
            if(!numTest){return;}
            this.status="ZIP length must be 5";
            if(!lenTest){return;}
            this.status="Enter ZIP between 00001 and 99950";
            if(!ranTest){return;}
            return true;
        },
        // Get zip inputted by user
        logZip(e){
            // Event gets sent as a parameter
            // We get the previous siblings value which is the input text
            var tempZipcode = e.target.previousSibling.value
            var notification = document.getElementById('notification_container');
            var notify_color = document.getElementById('notification');
            var notify_text = document.getElementById('notification_text');

            // Before updating the data variable with the new zipcode 
            // We have to validate, make sure its a number, and a valid zipcode
            if(this.validateZip(tempZipcode)){
                // inputted ZIP code passed initial validation
                this.zipcode = tempZipcode;
                this.key = "user";
                this.callAPI(this.zipcode, this.key, notify_color,notify_text,notification);
            }
            else{
                this.notifyError(notify_color,notify_text,notification);
            }
        },
        // Show and explain error for not finding ZIP
        notifyError(color,text,notification){
            // if we don't receive the element, return to prevent error in console
            if(color==undefined){return;}
            color.style.backgroundColor = "rgb(189, 33, 22)";
            text.innerHTML = this.status;
            notification.style.marginTop = "0px";
            notification.style.opacity = "1";
            setTimeout(() => { 
                notification.style.marginTop = "-30px";
                notification.style.opacity = "0";
            }, 1500);
        },
        // Show a successful notification of ZIP being found
        notifySuccess(color,text,notification){
            // if we don't receive the element, return to prevent error in console
            if(color==undefined){return;}
            color.style.backgroundColor = "rgb(22, 189, 58)";
            text.innerHTML = this.status;
            notification.style.marginTop = "0px";
            notification.style.opacity = "1";
            setTimeout(() => { 
                notification.style.marginTop = "-30px";
                notification.style.opacity = "0";
            }, 1500);  
        }
    }
}
</script>

<style scoped>
#user_container{
    position: relative;
    width: 100%;
    height: 25%;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
}
#textinput_container{
position: relative;
height: 50%;
width: 100%;
display: flex;
justify-content: center;
align-items: center;
}
#img_text{
background: linear-gradient(0deg, rgb(21, 108, 221), rgb(34, 180, 247));
width: 40%;
height: 40%;
border-radius: 100px;
border: 1px solid rgb(0, 183, 255);
display: flex;
align-items: center;
}
#text_input{
font-size: 20px;
text-align: center;
background: inherit;
border: none;
outline: none;
position: relative;
width: 72%;
height: 100%;
padding: 0px 0px 0px 10%;
border-top-left-radius: 200px;
border-bottom-left-radius: 200px;
color: rgba(255,255,255,0.85)
}
#text_input::placeholder {
color: white;
opacity: 0.75; /* Firefox */
}
#zip_image{
width: 23%;
margin-right: 5%;
filter: brightness(1%);
}
#zip_image:hover{
cursor: pointer;
filter: brightness(0%) invert(100%);
/* filter: invert(55%) sepia(65%) saturate(4956%) hue-rotate(173deg) brightness(90%) contrast(89%); */
}
#btn_container{
display: flex;
width: 100%;
height: 50%;
justify-content: center;
align-items: start;
}
.btn{
padding-left: 5px;
position: relative;
border-radius: 50%;
aspect-ratio: 1 / 1;
height: 50px;
margin: 0px 10% 0px 10%;
border: none;
cursor: pointer;
color: rgb(18, 184, 226);
display: flex;
justify-content: center;
align-items: center;
font-size: 20px;
transition: all 0.3s ease;
border: 1px solid rgb(18, 184, 226);
}
#btn_c{
background: transparent;
}
#btn_f{
background: linear-gradient(25deg,rgb(18, 184, 226), rgb(16, 104, 219));
color: black;
}



#home_bar{
position: absolute;
width: 35%;
height: 7px;
border-radius: 10px;
background: rgba(255,255,255,0.5);
bottom: 0;
left: 32.5%;
margin-bottom: 8px;
transition: all 0.3s ease;
}
#home_bar:hover{
background: rgba(255,255,255,0.9);
margin-bottom: 15px;
}
</style>
