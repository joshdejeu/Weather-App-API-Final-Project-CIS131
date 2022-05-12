<template>
    <div id="user_container">
        <div id="textinput_container">
            <div id="img_text">
                <input id="text_input" type="text" placeholder="Enter ZIP">
                <img src="../assets/pin.png" id="zip_image" @click="searchZip($event)"/>
            </div>
        </div>
        
        <div id="btn_container">
            <input class="btn" id="btn_c" type="button" value="C" @click="changeScale('C')">
            <input class="btn" id="btn_f" type="button" value="F" @click="changeScale('F')">
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
        activescale: "F",
        zipcode: 12345,



        city: "Minsk",
        temp: 21,
        type: "Thunderstorm",
        date: "Tuesday, May 5th",
        wind: 0,
        humidity: 0,
        pressure: 0,
        keyAPI: "b14a0f6f86b259ba7073b364662a0387",
        countrycode: "US",
        unit: "imperial",
        // if the random zip generated doesn't have data we use this to generate again until a valid number appears
        randomGenWorks: true,
    };
  },
  props:{

  },
  methods:{
      callAPI(){
            // Here we call the API to get data from their servers with updated ZIP
            var link = `https://api.openweathermap.org/data/2.5/weather?zip=${this.zipcode},${this.countrycode}&appid=${this.keyAPI}&units=${this.unit}`
            axios.get(link)
            .then(response => {
                // JSON responses are automatically parsed.
                // console.log("Success\n",response);
                this.city= response.data.name;
                this.temp= response.data.main.temp.toFixed(0);
                this.wind= response.data.wind.speed;
                this.humidity= response.data.main.humidity;
                this.pressure= response.data.main.pressure;
                this.type= response.data.weather[0].description;
                // if(this.randomGenWorks == false){this.randomGenWorks = true;}
            })
            .catch(e => {
                console.log("ZIP was not found, changing class to False")
                console.log(this.$refs.userInput)
                var i_care = false;
                if(i_care){
                    console.log("Error\n",e);
                }
                
                
                // if(this.randomGenWorks == true){
                //     this.randomGenWorks = false;
                //     this.setRandomZip();
                // }
            })
      },
      changeScale(val){
          this.activescale = val;
          if(this.activescale == "F"){
              console.log('F')
          }else{
              console.log('C')
          }
      },
      // Checks for a valid zip code
      validateZip(zipcode){
            var nonTest = zipcode.length==0;
            var numTest = !isNaN(zipcode);
            var lenTest = zipcode.length==5;
            var ranTest = parseInt(zipcode)>0 && parseInt(zipcode)<99951;
            // !isNaN() returns true if is a number
            // .length == 5 returns true if exactly 5 numbers
            // 0>x<99951 because valid US zipcodes are between 00001 and 99950
            // We will check each statemenet individually to update the error message and get useful info to the user
            this.notifyMsg="ZIP entry can not be empty";
            if(nonTest){return;}
            this.notifyMsg="ZIP must be all numbers";
            if(!numTest){return;}
            this.notifyMsg="ZIP length must be 5";
            if(!lenTest){return;}
            this.notifyMsg="Enter ZIP between 00001 and 99950";
            if(!ranTest){return;}
            this.notifyMsg="ZIP code Successfully Found";
            return true;
      },
      // Notification for success or fail
      searchZip(e){
            // Event gets sent as a parameter
            // We get the previous siblings value which is the input text
            var tempZipcode = e.target.previousSibling.value
            var notification = document.getElementById('notification_container');
            var notify_color = document.getElementById('notification');
            var notify_text = document.getElementById('notification_text');
            // Before updating the data variable with the new zipcode 
            // We have to validate, make sure its a number, and a valid zipcode
            // If API is successful the className of this component DIV will be "True"
            // IF API is unseccessful the className of this component DIV will be "False"
            if(this.validateZip(tempZipcode)){
                this.callAPI(this.zipcode);





                // Above is the new code








                // send the zipcode to API
                // this.$emit('emit_start', this.zipcode);
                // check class name of this component parent div to see if API passed or failed
                setTimeout(() => {
                    var APIpass = document.getElementById('user_container').className;
                    console.log("2nd Page",APIpass) 
                }, 2500);


                console.log('passed validation, sending to API')
                this.zipcode = tempZipcode;
                // We communicate with the parent commponent using emitters, passing the current zipcode as a param
                // if(APIpass=="false"){break;}
                // Show a succesful notification then make it disappear after 1.5s
                notify_color.style.backgroundColor = "rgb(22, 189, 58)";
                notify_text.innerHTML = this.notifyMsg;
                notification.style.marginTop = "0px";
                notification.style.opacity = "1";
                setTimeout(() => { 
                    notification.style.marginTop = "-30px";
                    notification.style.opacity = "0";
                }, 1500);  
            }
            else{
                var APIpass = document.getElementById('user_container').className;
                notify_color.style.backgroundColor = "rgb(189, 33, 22)";
                // If API fails, update the notification to indicate this
                if(APIpass=="false"){this.notifyMsg = "API has no data for this ZIP";}
                notify_text.innerHTML = this.notifyMsg;
                notification.style.marginTop = "0px";
                notification.style.opacity = "1";
                setTimeout(() => { 
                    notification.style.marginTop = "-30px";
                    notification.style.opacity = "0";
                }, 1500);
            }
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
  opacity: 1; /* Firefox */
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
    position: relative;
    border-radius: 50%;
    aspect-ratio: 1 / 1;
    height: 50px;
    margin: 0px 10% 0px 10%;
    border: none;
    cursor: pointer;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;
    transition: all 0.3s ease;
}
#btn_c{
    background: rgb(30, 80, 231);
    box-shadow: 0px 0px 10px rgba(23, 116, 255, 0.8), -3px 4px 0px rgb(56, 32, 194);
}
#btn_f{
    background: rgb(212, 144, 55);
    box-shadow: 0px 0px 10px rgba(255, 116, 23, 0.8), -3px 4px 0px rgb(112, 91, 22);
}
#btn_f:hover{
    box-shadow: 0px 0px 12px rgba(255, 255, 255,0.8);
}
#btn_c:hover{
    box-shadow: 0px 0px 12px rgba(255, 255, 255,0.8);
}

#btn_f:active{
    box-shadow: 0px 0px 10px rgba(255, 116, 23, 0.8), 0px 0px 0px rgb(112, 91, 22), inset 0px 0px 10px 5px rgba(0, 0, 0, 0.692);
}
#btn_c:active{
    box-shadow: 0px 0px 10px rgba(23, 116, 255, 0.8),  0px 0px 0px rgb(56, 32, 194), inset 0px 0px 10px 5px rgba(0, 0, 0, 0.692);
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
