<script setup>
window.addEventListener("keypress", (event)=> {
  let display= document.querySelector('h1')
  let projectile= document.querySelector('#ball')
  let leftPaddle=document.querySelector('.left')
  let rightPaddle=document.querySelector('.right')
  let width=window.innerWidth
  let height=window.innerHeight
  let key=event.key
if(key== 'w'){
leftPaddle.style=`
top:${leftPaddle.getBoundingClientRect().y +1 +'px' }
`
}
else if(key== 's'){
  leftPaddle.style=`
top:${leftPaddle.getBoundingClientRect().y -1 +'px' }
`
}
}
  );
function sendUserData(){
let startTime= Date.now()
}
  
</script>

<template>
  <h1 @click="sendUserData">score</h1>
  <div class="bodyContainer">
    <div class="paddle left"></div>
    <div id="ball"></div>
    <div class="paddle right"></div>
  </div>
</template>

<style >
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
::-webkit-scrollbar {
display: none;
}
html{
  width: 100vw;
  height: 100vh;
}
.bodyContainer{
  width: 100vw;
  height: 100vh;
}
.paddle{
  width: 1em;
  height: 7vw;
  background-color: red;
}
.left{
  position: fixed;
  left: 2%;
  top :50%;
  transform: translate(-50%,-50%);
}
.right{
  position: fixed;
  right: 2%;
  top :50%;
  transform: translate(-50%,-50%);
}
#ball{
  height: 3em;
aspect-ratio: 1;
border-radius: 50%;
  position :fixed;
  left: 50%;
  top :50%;
  transform: translate(-50%,-50%);
  background-image: radial-gradient(circle, #efefef, #c6c6c6, #9e9e9e, #797979, #555555);
}
</style>
