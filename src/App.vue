<template>
  <div id="app">
    <div id="phone_container">
      <div id="phone_back"></div>
      <div id="phone_right"></div>
      <div id="phone_left"></div>
      <div id="phone_top"></div>
      <div id="phone_bottom"></div>

      <div id="top_left"></div>
      <div id="bottom_left"></div>
      <div id="top_right"></div>
      <div id="bottom_right"></div>

      <PhoneFront/>
    </div>

    <div id="rotate_controls">
      X: {{X}}deg<input type="range" id="rotX" value="0" @input="changeRange">
      Y: {{Y}}deg<input type="range" id="rotY" value="0" @input="changeRange">
      Z: {{Z}}deg<input type="range" id="rotZ" value="0" @input="changeRange">
    </div>
  </div>
</template>

<script>
import PhoneFront from "./components/PhoneFront"

export default {
  name: 'App',
  components: {
    PhoneFront
  },
  data(){
    return{
      X: 0,
      Y: 0,
      Z: 0,
    }
  },
  methods:{
    // A.0) pull value from inputs
    changeRange(e){
      // A.1) range value max is 100 but rotate max is 360 so we multiply the value by 3.6
      this.X = (e.srcElement.offsetParent.children[0].value*3.6).toFixed(0);
      this.Y = (e.srcElement.offsetParent.children[1].value*3.6).toFixed(0);
      this.Z = (e.srcElement.offsetParent.children[2].value*3.6).toFixed(0);
      // A.2) not applying the rotations with the values provided
      var phone = document.getElementById('phone_container');
      phone.style.transform = `rotateX(${this.X}deg) rotateY(${this.Y}deg) rotateZ(-${this.Z}deg)`;
    }
  }
}
// Code by Josh D
// The structure of this project is a Phone Front component with an area to display data and area for user input
// The Phone Front component contains the 2nd and last component which is called User Input
// 
// Inside the User Input container it handles everything from:
//    1) validating zipcodes
//    2) calling the API
//    3) sending the PhoneContainer API responses
//    2) displaying successful and failed API calls
//    2) generating new random zipcodes on load
//    4) and options for Imperial or Metric measurements
// 
// Enjoy!
</script>

<style>
#phone_container{
    perspective: 100%;
    transform-style: preserve-3d;
    position: relative;
    height: 80vh;
    aspect-ratio: 0.50;
    transition: 0.5s ease;
    transform: rotateX(70deg) rotateY(0deg) rotateZ(0deg);
}
/* #phone_front{
} */
#phone_back{
    position: absolute;
    transform: translateZ(-25px);
    height: 100%;
    width: 100%;
    border-radius: 20%/10%;
    border: 10px solid black;
    box-shadow:     
    0px 0px 0px 2px #51657a,
    0px 0px 0px 3px #e1e5e8,
    0px 0px 0px 4px #67748e,
    0px 0px 0px 5px #bfc4c7;;
    /* background: #000918; */
    background: linear-gradient(0deg,#8ca1b5,#b7d5ef);
}
#phone_back::after{
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  transform: translateZ(300px);
  /* background-color: white; */
}
#phone_right{
  background-color: #67748e;
  background: linear-gradient(-130deg, rgb(99, 99, 100) 30%, #a4adb8 40%, #b2c1ce 50%, #67748e 60%);
  height: 85%;
  width: 24px;
  transform: translateZ(-12.5px) rotateY(90deg);
  position: absolute;
  top: 9%;
  left: calc(100% + 12px);
}
#phone_left{
  background-color: #67748e;
  background: linear-gradient(-130deg, rgb(102, 102, 105) 30%, #aabccc 40%, #b1bbc7 50%, #67748e 60%);
  height: 85%;
  width: 24px;
  transform: translateZ(-12.5px) rotateY(90deg);
  position: absolute;
  top: 9%;
  left: calc(0% - 12px);
}
#phone_top{
  background-color: #67748e;
  background: linear-gradient(130deg, rgb(92, 92, 94) 30%, #a8b3c5 40%, #adc3cf 50%, #67748e 60%);
  height: 24px;
  width: 70%;
  transform: translateZ(-12.5px) rotateX(90deg) ;
  position: absolute;
  top: -15px;
  left: 17%;
}
#phone_bottom{
  background-color: #67748e;
  background: linear-gradient(130deg, rgb(103, 103, 105) 30%, #9497a1 40%, #939aa3 50%, #67748e 60%);
  height: 24px;
  width: 70%;
  transform: translateZ(-12.5px) rotateX(90deg) ;
  position: absolute;
  bottom: -33px;
  left: 17%;
}
#bottom_left{
  content: '';
  background-color: #67748e;
  background: linear-gradient(130deg, rgb(103, 103, 105), #67748e);
  height: 24px;
  width: 95px;
  display: inline-block;
  transform: translateZ(-12.5px) rotateZ(45deg) rotateX(90deg);
  position: absolute;
  bottom: 0;
  left: -14px;
}
#bottom_right{
  content: '';
  background-color: #67748e;
  background: linear-gradient(45deg, rgb(103, 103, 105), #67748e);
  height: 24px;
  width: 95px;
  display: inline-block;
  transform: translateZ(-12.5px) rotateZ(-45deg) rotateX(90deg);
  position: absolute;
  bottom: 0;
  right: -30px;
}
#top_left{
  content: '';
  background-color: #67748e;
  background: linear-gradient(45deg, rgb(103, 103, 105), #67748e);
  height: 24px;
  width: 95px;
  display: inline-block;
  transform: translateZ(-12.5px) rotateZ(-45deg) rotateX(90deg);
  position: absolute;
  top: 20px;
  left: -14px;
}
#top_right{
  content: '';
  background-color: #67748e;
  background: linear-gradient(-0deg, rgb(103, 103, 105), #67748e);
  height: 24px;
  width: 95px;
  display: inline-block;
  transform: translateZ(-12.5px) rotateZ(45deg) rotateX(90deg);
  position: absolute;
  top: 20px;
  right: -30px;
}


#rotate_controls{
  display: flex;
  flex-direction: column;
  position: absolute;
  height: 10%;
  justify-content: space-around;
  top: 0;
  right: 0;
  padding: 10px;
  font-family: 'Segoe UI', sans-serif;
}


*{
  margin: 0;
  padding: 0;
}
html,body{
  width: 100%;
  height: 100%;
}
body{
  overflow: hidden;
  background: linear-gradient(135deg,#4f63fe, #3b9fcc , #05449e);
}
#app {
  width: 100%;
  height: 100%;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;

}






@import url('https://fonts.googleapis.com/css2?family=Roboto&family=Roboto+Condensed:wght@700&display=swap');
*{
  font-size: 1em;
}
#temp{
  font-family: 'Roboto Condensed', serif;
  font-weight: bold;
}
#other{
  font-family: 'Roboto', sans-serif;
  font-weight: 300;
}
</style>
