<script setup lang="ts">
import type { ImgSwitcher } from "~/components/ImageSwitcher.vue";
const prevRoomUserImg = ref("high-tech.jpeg");
const roomUserImg = ref("high-tech.jpeg");
const flagAnimateImageRoom = ref(false);
let timerId: number | null = null;

const roomImgVariables = computed(() => {
  const base = useRuntimeConfig().app.baseURL; 
  return {
    "--room-img": `url(${base}images/${roomUserImg.value})`,
    "--portrait-room-img": `url(${base}images/portrait-${roomUserImg.value})`,
    "--prev-room-img": `url(${base}images/${prevRoomUserImg.value})`,
    "--portrait-prev-room-img": `url(${base}images/portrait-${prevRoomUserImg.value})`,
  };
});

const handleChangeRoom = (room: ImgSwitcher) => {
  if (roomUserImg.value === room.img) return;

  if (!flagAnimateImageRoom.value) {
    prevRoomUserImg.value = roomUserImg.value;
    roomUserImg.value = room.img;
    flagAnimateImageRoom.value = true;

    timerId = window.setTimeout(() => {
      flagAnimateImageRoom.value = false;
    }, 2000);
  }
};

const roomsData = ref<ImgSwitcher[]>([
  { name: "Loft", img: "loft.jpg" },
  { name: "High tech", img: "high-tech.jpeg" },
  { name: "Scandinavian", img: "scandinavian.jpg" },
  { name: "Modern", img: "modern.jpeg" },
  { name: "Eco", img: "eco.jpeg" },
  { name: "Country", img: "country.jpg" },
  { name: "Japan", img: "japan.jpeg" },
  { name: "Classic", img: "classic.jpeg" },
  { name: "Home", img: "sky.jpeg" },
]);
</script>
<template>
  <div
    :style="roomImgVariables"
    class="page"
    :class="{ 'page--animate-img': flagAnimateImageRoom }"
  >
    <div class="page__rooms">
      <ImageSwitcher
        @click-img="handleChangeRoom"
        :active-img="roomUserImg"
        :images="roomsData"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>

@keyframes changeRoom-v2 {
  0% {opacity: 0;}
  
  60%   {opacity: 1; clip-path: polygon( 0  0, 20%  0, 20% 100%, 0%   100%); }
  70%  { clip-path: polygon(40% 0, 20%  0, 20% 100%, 40%  100%); }
  80%  { clip-path: polygon(40% 0, 60%  0, 60% 100%, 40%  100%); }
  90%  { clip-path: polygon(80% 0, 60%  0, 60% 100%, 80%  100%); }
  100%  { clip-path: polygon(80% 0, 100% 0, 100% 100%, 80% 100%); }
}
@keyframes changeRoom-v3 {
  0% { clip-path: polygon(80% 0, 100% 0, 100% 100%, 80% 100%); }
  100%  { clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%); }
}

.page {
  position: relative;
  min-height: 100vh;


  &::before,
  &::after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;

 
  }

  &::before {
    background-image: var(--room-img);
    z-index: -1;


    @media (max-width: 1550px) {
      background-image: var(--portrait-room-img);
    }
  }

  &::after {
    background-image: var(--prev-room-img);
    z-index: -2;

    @media (max-width: 1550px) {
      background-image: var(--portrait-prev-room-img);
    }
  }

  &--animate-img::before {
    animation:  changeRoom-v2 1s step-start, changeRoom-v3 .5s 1s ease-in;
  }

  &__rooms {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translate(-50%);
  }
}
</style>
