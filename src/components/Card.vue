<template>
  <div class="card" @click="debugImage">
    <a :href="anime.url" target="_blank">
      <img :src="anime.image_url" :alt="anime.title" />
    </a>
    <h3>{{ anime.title }}</h3>
  </div>
</template>
<script>
export default {
  props: ["anime"],

  setup() {
    const debugImage = (anime) => {
      console.log(anime.title);
    };

    return {
      debugImage,
    };
  },
};
</script>
<style lang="scss">
.card {
  // border: 1px solid red;
  width: 15rem;
  height: 40vh;
  transition: 0.4s;
  &:hover {
    transform: scale(1.1);
  }
}
</style>
