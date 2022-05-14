<template>
  <div id="app">
    <div id="phone_container">
      <div  id="phone_back"></div>
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
// The structure of this project is a Phone Container component with an area to display data
// The Phone Container component contains the 2nd and last component which is called User Input
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
    transform-style: preserve-3d;
    position: relative;
    height: 80vh;
    aspect-ratio: 0.50;
    transition: 0.5s ease;
    transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
}
/* #phone_front{
} */
#phone_back{
    position: absolute;
    transform: translateZ(-30px);
    width: 104%;
    height: 100%;
    border-radius: 20%/10%;
    border: 10px solid black;
    overflow: visible;
    /* background: #000918; */
    background: #051838;
}
#phone_back::after{
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  transform: translateZ(300px);
  /* background-color: white; */
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
