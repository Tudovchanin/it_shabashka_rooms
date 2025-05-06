<script setup lang="ts">
const runtimeConfig = useRuntimeConfig();
const baseURL = runtimeConfig.app.baseURL;

export type ImgSwitcher = {
  name: string;
  img: string;
};

type PropsImgSwitcher = {
  images:  ImgSwitcher[];
  activeImg?: string
}

const props = defineProps<PropsImgSwitcher>()

const emit = defineEmits(['click-img']);

const emitClickInTheRoom = ((img: ImgSwitcher) => {
  emit('click-img', img)
})

</script>
<template>
  <div class="image-switcher">
    <div class="image-switcher__container">
      <div v-for="img in images" class="image-switcher__wrapper-btn" :key="img.name">
        <button @click="emitClickInTheRoom(img)"  class="image-switcher__btn"
          :class="{ 'image-switcher__btn--active': props.activeImg === img.img }">
          <img :src="`${baseURL}images/min-${img.img}`" :alt="img.name" />
        </button>
        <div class="image-switcher__btn-title">
          {{ img.name }}
        </div>

      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.image-switcher {
  padding: 20px;
  border: solid 1px white;
  text-align: center;
  background-color: rgba(0, 0, 0, 0.693);
  color: white;
 

  &__container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 40px;

    @media (max-width: 750px) {
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }

    @media (max-width: 550px) {
      gap: 15px;
    }
  }

  &__btn {
      width: 200px;
    height: 100px;
    border: none;
    margin-bottom: 10px;
    background-color: transparent;
    opacity: .4;
    cursor: pointer;

    & img {
      width: 100%;
      height: 100%;
    }

    @media (max-width: 550px) {
      width: 150px;
      height: 75px;
    }

    @media (max-width: 380px) {
      width: 100px;
      height: 50px;
    }

    @media (hover:hover) {
      transition: opacity 1s;

      &:hover {
        opacity: 8;
      }

      &:hover + .room__title {
        color: gray;
        text-decoration: rgb(255, 255, 255) underline solid;
      }
    }

    &--active {
      opacity: 1;
      outline: solid white 2px;
      pointer-events: none;
    }
  }

  &__btn-title {
    font-size: 20px;
    font-weight: 700;
    text-decoration: gray underline solid;

    @media (max-width: 380px) {
      font-size: 16px;
    }

    @media (hover:hover) {
      transition: color .5s ease-out, text-decoration .5s ease-in;
    }
  }
}

.room {
  &__title {
    font-size: 20px;
    font-weight: 700;
    text-decoration: gray underline solid;

    @media (max-width: 380px) {
      font-size: 16px;
    }

    @media (hover:hover) {
      transition: color .5s ease-out, text-decoration .5s ease-in;
    }
  }

  &__button {
    width: 200px;
    height: 100px;
    border: none;
    margin-bottom: 10px;
    background-color: transparent;
    opacity: .4;
    cursor: pointer;

    & img {
      width: 100%;
      height: 100%;
    }

    @media (max-width: 550px) {
      width: 150px;
      height: 75px;
    }

    @media (max-width: 380px) {
      width: 100px;
      height: 50px;
    }

    @media (hover:hover) {
      transition: opacity 1s;

      &:hover {
        opacity: 8;
      }

      &:hover + .room__title {
        color: gray;
        text-decoration: rgb(255, 255, 255) underline solid;
      }
    }

    &--active {
      opacity: 1;
      outline: solid white 2px;
      pointer-events: none;
    }
  }
}
</style>
