<template>
  <div id="sky" class="sky">
    <div ref="stars" v-for="star of params.amount" :key="star"></div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";

const params = ref({
  amount: 150,
  size: {
    min: 0.5,
    max: 1.5,
    giant: 2,
  },
  duration: {
    min: 2,
    max: 20,
  },
});

const stars = ref();

const randomBetween = (a: number, b: number) => {
  return a + Math.random() * (b - a);
};

const allStars = () => {
  stars.value.forEach((star: HTMLElement) => {
    starStyle(star);
  });
};

const starStyle = (star: HTMLElement) => {
  const size =
    Math.round(Math.random() * 10) === 0
      ? params.value.size.giant
      : randomBetween(params.value.size.min, params.value.size.max);
  star.style.width = size + "px";
  star.style.height = size + "px";
  star.style.position = "absolute";
  star.style.left = randomBetween(0, 100) + "%";
  star.style.top = randomBetween(0, 100) + "%";
  star.style.background =
    "radial-gradient(ellipse at center,#dce5ed 2%,#6c89a4 100%)";
  star.style.borderRadius = "100%";
  star.style.boxShadow = "0 0 " + size + "px " + size / 3 + "px #dce5ed";
  star.style.transform = "translate3d(0, 0, 200px)";
  star.style.animation = "shine infinite alternate";
  star.style.animationDuration =
    randomBetween(params.value.duration.min, params.value.duration.max) + "s";
};

const starMove = () => {
  if (stars.value.length > 0) {
    const star = stars.value[Math.floor(randomBetween(0, params.value.amount))];
    star.style.animation = "move linear";
    star.style.animationDuration = randomBetween(0.5, 1) + "s";
    setTimeout(() => {
      starStyle(star);
      starMove();
    }, Math.floor(randomBetween(3, 10)) * 1000);
  }
};

onMounted(() => {
  allStars();
  starMove();
});
</script>

<style scoped>
.sky {
  min-height: 100%;
  min-width: 100%;
  position: fixed;
  left: 0;
  top: 0;
  background: radial-gradient(ellipse at center, #08192a 10%, black 100%);
}
</style>
