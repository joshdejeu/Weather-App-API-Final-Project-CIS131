<template>
  <div id="app">
    <PhoneContainer/>
  </div>
</template>

<script>
import PhoneContainer from "./components/PhoneContainer"

export default {
  name: 'App',
  components: {
    PhoneContainer
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
