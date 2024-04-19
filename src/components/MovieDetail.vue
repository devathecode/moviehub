<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import LoaderComp from "./LoaderComp.vue"

const movie = ref({});
const route = useRoute();
const showLoader = ref([]);
const genreArr = ref([]);
const actorsArr = ref([]);
showLoader.value = true;
onMounted(() => {
  showLoader.value = true;
  fetch(`https://www.omdbapi.com/?apikey=8c8a07b&i=${route.params.id
    } & plot=full`)
    .then(response => response.json())
    .then(data => {
      movie.value = data;
      showLoader.value = false;
      if (data?.Genre) genreArr.value = data.Genre.split(',')
      if (data.Actors) actorsArr.value = data.Actors.split(',')
    });

  return {
    movie,
    showLoader,
    genreArr,
    actorsArr
  }
})
</script>

<template>
  <div v-if="showLoader">
    <LoaderComp />
  </div>
  <div v-else>
    <div class="grid grid-cols-12 gap-3 my-4 max-w-4xl mx-auto text-white pt-5">
      <div class="col-span-12 my-auto place-self-center sm:col-span-6 md:col-span-3">
        <img :src="movie.Poster" alt="" class="h-full w-full" />
      </div>
      <div class="col-span-12 p-2 sm:col-span-6">
        <h1 class="mb-3 text-4xl font-semibold">{{ movie.Title }}</h1>
        <p class="italic">{{ movie.Released }}</p>
        <p class="mt-3"><b>Runtime:</b> {{ movie.Runtime }}</p>
        <div class="flex mt-3" v-if="genreArr">
          <b>Genre:</b>
          <div class="ms-2 flex flex-wrap space-x-2" v-for="item in genreArr" :key="item">
            <div class="h-6 rounded-md bg-yellow-600 px-2 text-white me-2">{{ item }}</div>
          </div>
        </div>

        <div class="flex mt-3" v-if="actorsArr">
          <b>Actors:</b>
          <div class="ms-2 flex flex-wrap space-x-2" v-for="item in actorsArr" :key="item">
            <div class="h-6 rounded-md bg-yellow-600 px-2 text-white me-2">{{ item }}</div>
          </div>
        </div>
        <p class="mt-3"><b>Director:</b> {{ movie.Director }}</p>
        <div class="mt-3 flex">
          <h2 class="me-2">Ratings:</h2>
          <p class="me-2"><b>IMDB:</b> {{ movie.imdbRating }}/10</p>
          <p><b>Metacritic:</b> {{ movie.Metascore }}/100</p>
        </div>
      </div>
      <div class="col-span-12 row-span-2 p-2 sm:col-span-3">
        <p class="mt-20"><b>Box Office:</b> {{ movie.BoxOffice }}</p>
        <p v-if="movie.Awards"><b>Awards:</b> {{ movie.Awards }}</p>
      </div>
      <div class="col-span-12 p-2 sm:col-span-9">
        <p><b>Plot Summary:</b>{{ movie.Plot }}</p>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
$color_1: rgb(255 255 255);
$background-color_1: rgb(0 0 0 / 0.6);
$background-color_2: rgb(202 138 4);

.pt-5 {
  padding-top: 5rem;
}

.w-full {
  width: 100%;
}

.max-w-4xl {
  max-width: 70rem;
}

.col-span-12 {
  grid-column: span 12 / span 12;
}

.row-span-2 {
  grid-row: span 2 / span 2;
}

.my-auto {
  margin-top: auto;
  margin-bottom: auto;
}

.mx-auto {
  margin-left: auto;
  margin-right: auto;
}

.mb-3 {
  margin-bottom: 0.75rem;
}

.my-4 {
  margin-bottom: 1rem;
  margin-top: 1rem;
}

.me-2 {
  margin-inline-end: 0.5rem;
}

.ms-2 {
  margin-inline-start: 0.5rem;
}

.mt-20 {
  margin-top: 5rem;
}

.mt-3 {
  margin-top: 0.75rem;
}

.flex {
  display: flex;
}

.grid {
  display: grid;
}

.h-6 {
  height: 1.5rem;
}

.h-full {
  height: 100%;
}

.w-\[30rem\] {
  width: 30rem;
}

.grid-cols-12 {
  grid-template-columns: repeat(12, minmax(0, 1fr));
}

.flex-wrap {
  flex-wrap: wrap;
}

.gap-3 {
  gap: 0.75rem;
}

.place-self-center {
  place-self: center;
}

.rounded-md {
  border-radius: 0.375rem;
}

.bg-black\/60 {
  background-color: $background-color_1;
}

.bg-yellow-600 {
  background-color: $background-color_2;
}

.bg {
  background: no-repeat fixed url('https://i.postimg.cc/PJ4P97Vr/moviebanner.webp');
}

.bg-center {
  background-size: cover;
}

.p-2 {
  padding: 0.5rem;
}

.px-2 {
  padding-left: 0.5rem;
  padding-right: 0.5rem;
}

.text-center {
  text-align: center;
}

.text-4xl {
  font-size: 2.25rem;
  line-height: 2.5rem;
}

.font-semibold {
  font-weight: 600;
}

.italic {
  font-style: italic;
}

.text-white {
  color: $color_1;
}

.underline {
  text-decoration-line: underline;
}

.decoration-yellow-400 {
  text-decoration-color: #facc15;
}

.underline-offset-4 {
  text-underline-offset: 4px;
}

.bg-blend-multiply {
  background-blend-mode: multiply;
}

@media (min-width: 640px) {
  .sm\:col-span-3 {
    grid-column: span 3 / span 3;
  }

  .sm\:col-span-6 {
    grid-column: span 6 / span 6;
  }

  .sm\:col-span-9 {
    grid-column: span 9 / span 9;
  }
}

@media (min-width: 768px) {
  .md\:col-span-3 {
    grid-column: span 3 / span 3;
  }
}
</style>