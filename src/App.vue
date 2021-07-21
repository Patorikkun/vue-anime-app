<template>
  <div class="app">
    <header>Weeb<strong>DB</strong></header>

    <form class="search-box" @submit.prevent="HandleSearch">
      <input
        type="search"
        class="search-field"
        placeholder="Lookin for somethin, weeb?"
        v-model="search_query"
        required
      />
    </form>
    <main>
      <div class="cards" v-if="animelist.length > 0">
        <Card v-for="anime in animelist" :key="anime.mal_id" :anime="anime" />
      </div>
      <div class="no-results" v-else></div>
      <h3>Couldn't find whatcha lookin for</h3>
    </main>
  </div>
</template>

<script>
import { ref } from "vue";

import Card from "./components/Card.vue";
export default {
  components: { Card },
  setup() {
    const search_query = ref("");
    const animelist = ref([]);

    const HandleSearch = async () => {
      animelist.value = await fetch(
        `https://api.jikan.moe/v3/search/anime?q=${search_query.value}`
      )
        .then((res) => res.json())
        .then((data) => data.results);

      console.log(animelist.value);

      //pass the searched anime
      search_query.value = "";
    };

    return {
      Card,
      search_query,
      animelist,
      HandleSearch,
    };
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;

  font-family: "Fira Sans", sans-serif;
}

header {
  font-size: 5rem;
  // border: 1px solid red;
  margin-bottom: 1rem;
}
.app {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  height: 100vh;
}
.search-box {
  //  border: 1px solid red;
  width: 60%;
  .search-field {
    padding: 0 2.5rem;
    font-size: 2rem;
    height: 10vh;
    width: 100%;
    border: none;
    background-color: whitesmoke;
    border-radius: 1rem;
    outline: none;
  }
}

main {
  // border: 1px solid green;
  height: 70vh;
  width: 90%;
  margin: auto;
  padding: 30px 0;
}

.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr));
  gap: 3.5rem;

  img {
    width: 100%;
    height: 80%;
    border-radius: 1rem;
    margin-bottom: 1rem;
    filter: drop-shadow(0 0.5rem 0.25rem rgb(192, 192, 192));
  }
}
</style>
