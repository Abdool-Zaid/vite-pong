<script setup>
import { ref, onMounted } from "vue";

const el = ref();

onMounted(() => {
  let display = document.querySelector("h1");
  let projectile = document.querySelector("#ball");
  let leftPaddle = document.querySelector(".left");
  let width = window.innerWidth;
  let height = window.innerHeight;
  let initialDirection = 0;
  let displacement = "1px";
  let rightPaddle = document.querySelector(".right");
  let startTime = Date.now();

  window.addEventListener("keypress", (event) => {
    let key = event.key;
    if (key == "q" && leftPaddle.getBoundingClientRect().y > 0) {
      leftPaddle.style = `
      top:${leftPaddle.getBoundingClientRect().y + displacement};
      `;
    } else if (
      key == "a" &&
      leftPaddle.getBoundingClientRect().bottom < height
    ) {
      leftPaddle.style = `
      top:${leftPaddle.getBoundingClientRect().bottom + displacement};
      `;
    } else if (key == "p" && rightPaddle.getBoundingClientRect().y > 0) {
      rightPaddle.style = `
    top:${rightPaddle.getBoundingClientRect().y + displacement};
    `;
    } else if (
      key == "l" &&
      rightPaddle.getBoundingClientRect().bottom < height
    ) {
      rightPaddle.style = `
    top:${rightPaddle.getBoundingClientRect().bottom + displacement};
    `;
    } else if (key == " ") {
      fetch("https://yesno.wtf/api")
        .then((res) => res.json())
        .then((data) => {
          if (data.answer == "no") {
            initialDirection = -1;
          } else {
            initialDirection = 1;
          }
        });
    }
  });
  function sendUserData() {
    if (projectile.getBoundingClientRect().bottom < height && projectile.getBoundingClientRect().y > 0) {
      projectile.style = `
        top:${projectile.getBoundingClientRect().bottom + displacement};
        `;
      }
      console.log(projectile.getBoundingClientRect().bottom, width);
    setTimeout(sendUserData, 1000);
  }
  sendUserData();
});
</script>

<template>
  <h1 @click="sendUserData">score</h1>
  <div class="bodyContainer">
    <div class="paddle left"></div>
    <div id="ball"></div>
    <div class="paddle right"></div>
  </div>
</template>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
::-webkit-scrollbar {
  display: none;
}
html {
  position: fixed;
  top: 0;
  left: 0;
  margin: 1em;
  width: 100vw;
  height: 100vh;
}
.bodyContainer {
  width: 100vw;
  height: 100vh;
}
.paddle {
  width: 1em;
  height: 7vw;
  background-color: red;
}
.left {
  position: fixed;
  left: 2%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.right {
  position: fixed;
  right: 2%;
  top: 50%;
  transform: translate(-50%, -50%);
}
#ball {
  height: 3em;
  aspect-ratio: 1;
  border-radius: 50%;
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background-image: radial-gradient(
    circle,
    #efefef,
    #c6c6c6,
    #9e9e9e,
    #797979,
    #555555
  );
}
</style>
