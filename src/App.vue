<script setup>
import { ref, onMounted } from "vue";
const el = ref();
onMounted(() => {
  let display = document.querySelector("h1");
  let projectile = document.querySelector("#ball");
  let leftPaddle = document.querySelector(".left");
  let width = window.innerWidth;
  let intialY;
  let progressiveDirectionX;
  let height = window.innerHeight;
  let initialDirection;
  let rankSpeed = 100;
  let displacement = 1;
  let rightPaddle = document.querySelector(".right");
  if (Math.round(Math.random() * 10) < 5) {
    intialY = () => {
      return projectile.getBoundingClientRect().bottom + displacement + "px";
    };
  } else {
    intialY = () => {
      return projectile.getBoundingClientRect().y + displacement + "px";
    };
  }
  function strikeProjectile() {
    if (
      projectile.getBoundingClientRect().y <
        leftPaddle.getBoundingClientRect().bottom &&
      projectile.getBoundingClientRect().y >
        leftPaddle.getBoundingClientRect().top
    ) {
      if (
      projectile.getBoundingClientRect().left <
      leftPaddle.getBoundingClientRect().left
    ) {
      progressiveDirectionX = () => {
        return projectile.getBoundingClientRect().right + displacement + "px";
      };
    } else if (
      projectile.getBoundingClientRect().left >
      rightPaddle.getBoundingClientRect().right
    ) {
      progressiveDirectionX = () => {
        return projectile.getBoundingClientRect().left + displacement + "px";
      };
    }
    }
    return progressiveDirectionX;
  }
  function checkBoundary() {
    if (
      projectile.getBoundingClientRect().top > 0 &&
      projectile.getBoundingClientRect().bottom < height
    ) {
      return intialY();
    } else {
      let transientValue = String(intialY);
      if (transientValue.includes("bottom")) {
        intialY = () => {
          return projectile.getBoundingClientRect().y + displacement + "px";
        };
        return intialY;
      } else {
        intialY = () => {
          return (
            projectile.getBoundingClientRect().bottom + displacement + "px"
          );
        };
        return intialY;
      }
    }
  }
  window.addEventListener("keypress", (event) => {
    let key = event.key;
    if (key == "z" && leftPaddle.getBoundingClientRect().y > 0) {
      leftPaddle.style = `
      top:${leftPaddle.getBoundingClientRect().y + displacement + "px"};
      `;
      rightPaddle.style = `
    top:${rightPaddle.getBoundingClientRect().y + displacement + "px"};
    `;
    } else if (
      key == "x" &&
      leftPaddle.getBoundingClientRect().bottom < height
    ) {
      leftPaddle.style = `
      top:${leftPaddle.getBoundingClientRect().bottom + displacement + "px"};
      `;
      rightPaddle.style = `
    top:${rightPaddle.getBoundingClientRect().bottom + displacement + "px"};
    `;
    } else if (key == " ") {
      let startTime = Date.now();
      rankSpeed > 1 ? rankSpeed - 10 : rankSpeed;

      let intialSetting = Math.round(Math.random() * 10);
      if (
        Math.round(Math.random() * 10) > 5 ||
        projectile.getBoundingClientRect().y ==
          leftPaddle.getBoundingClientRect().y
      ) {
        initialDirection = () => {
          return projectile.getBoundingClientRect().right + displacement + "px";
        };
      } else if (
        Math.round(Math.random() * 10) < 5 ||
        projectile.getBoundingClientRect().y ==
          rightPaddle.getBoundingClientRect().y
      ) {
        initialDirection = () => {
          return projectile.getBoundingClientRect().left + displacement + "px";
        };
      }
      function sendUserData() {
        display.innerHTML = `
      score: ${(Date.now() - startTime) / 100}`;
        progressiveDirectionX = initialDirection();
        if (
          projectile.getBoundingClientRect().left < width &&
          projectile.getBoundingClientRect().right > 0
        ) {
          projectile.style = `
          left:${strikeProjectile()};
          top:${checkBoundary()}
            `;
        } else {
          // alert(display.innerHTML);
          window.location.reload();
        }
        setTimeout(sendUserData, rankSpeed);
      }
      sendUserData();
    }
  });
});
</script>

<template>
  <h1 @click="sendUserData">score</h1>
  <div class="bodyContainer">
    <div class="paddle left"></div>
    <div class="paddle center left"></div>
    <div id="ball"></div>
    <div class="paddle right"></div>
  </div>
</template>

<style>

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
  height: 20vw;
  background-color: red;
}
.left {
  position: fixed;
  left: 2%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.center{
  z-index: 10;
  position: fixed;
  left: 50%;
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
