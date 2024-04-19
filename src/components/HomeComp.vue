
<script setup>
import { onMounted, ref } from 'vue';
import LoaderComp from "./LoaderComp.vue"

const search = ref("");
const movies = ref([]);
const showLoader = ref([]);

const searchKeyArr = ['train', 'harry', 'man', 'evil', 'master']

onMounted(() => {
  showLoader.value = true;
  fetch(`http://www.omdbapi.com/?apikey=8c8a07b&s=${searchKeyArr[(Math.floor(Math.random() * searchKeyArr.length))]}`)
    .then(response => response.json())
    .then(data => {
      movies.value = data.Search;
      showLoader.value = false;
      search.value = "";
    });
})
const reff = ref();

const SearchMovies = () => {
  showLoader.value = true;
  if (search.value != "") {
    fetch(`http://www.omdbapi.com/?apikey=8c8a07b&s=${search.value}`)
      .then(response => response.json())
      .then(data => {
        movies.value = data.Search;
        showLoader.value = false
        search.value = "";
      });
  }
}
</script>
<template>
  <div id="home">
    <div class="hero-section">
      <img src="https://i.postimg.cc/PJ4P97Vr/moviebanner.webp" alt="Naruto Poster" class="hero-img" />
      <div class="detail flex flex-col text-center">
        <h1><span>Dev</span>Flix</h1>
        <h2>Unlimited movies, TV shows and more</h2>
        <p>
          Watch anywhere. Cancel anytime.
          <br> <br>
          Ready to watch? Enter your email to create or restart your membership.
        </p>
        <form @submit.prevent="SearchMovies()" class="search-box">
          <input type="text" placeholder="What are you looking for?" v-model="search" />
          <button type="submit" value="">
            <svg class="svg-icon search-icon" aria-labelledby="title desc" role="img" xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 19.9 19.7">
              <title id="title">Search Icon</title>
              <desc id="desc">A magnifying glass icon.</desc>
              <g class="search-path" fill="none" stroke="white">
                <path stroke-linecap="square" d="M18.5 18.3l-5.4-5.4" />
                <circle cx="8" cy="8" r="7" />
              </g>
            </svg>
          </button>
        </form>
      </div>
    </div>

    <div v-if="showLoader">
      <LoaderComp />
    </div>
    <div class="grid" ref="reff" id="targetDiv" v-else>
      <article v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID">
          <img :src="movie.Poster" alt="Movie Poster" />
          <div class="text">
            <div class="type">{{ movie.Type }}</div>
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>
      </article>
    </div>
  </div>
</template>

<style lang="scss" scoped>
$breakpoint-mobile: 576px;
$breakpoint-tablet: 768px;
$breakpoint-desktop: 1024px;

@mixin cover-image($width, $height) {
  display: block;
  width: $width;
  height: $height;
  object-fit: cover;
  position: relative;
}

@mixin position($position, $top: 0, $right: 0, $bottom: 0, $left: 0) {
  position: $position;
  top: $top;
  right: $right;
  bottom: $bottom;
  left: $left;
}

@mixin flex($justify-content: center, $align-items: center) {
  display: flex;
  justify-content: $justify-content;
  align-items: $align-items;
}

@mixin input-reset {
  appearance: none;
  border: none;
  outline: none;
  background: none;
}

@mixin grid($columns: repeat(auto-fill, minmax(20rem, 2fr)), $rows: minmax(200px, auto), $gap: 20px, $align-items: stretch, $padding: 2rem 5vw) {
  display: grid;
  grid-template-columns: $columns;
  grid-template-rows: $rows;
  grid-gap: $gap;
  align-items: $align-items;
  padding: $padding;
}

#home {
  .hero-section {
    position: relative;

    .hero-img {
      @include cover-image(100%, 53.7vh);
      z-index: 0;

      @media (max-width: $breakpoint-tablet) {
        @include cover-image(100%, 60vh);
      }
    }

    .detail {
      @include position(absolute, 0);
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      padding-top: 5rem;
      z-index: 1;

      h1 {
        text-align: center;
        color: #FFF;
        font-size: 5rem;
        margin-bottom: 1rem;

        @media (max-width: $breakpoint-tablet) {
          font-size: 3rem;
        }

        span {
          color: #ecd23a;
        }
      }

      h2 {
        color: #FFF;
        margin-bottom: 16px;
        font-size: 3rem;

        @media (max-width: $breakpoint-tablet) {
          font-size: 1.5rem;
        }
      }

      p {
        color: #FFF;
        margin-bottom: 3rem;
      }
    }
  }

  .search-box {
    @include flex(center, center);
    padding: 16px;
    width: 100%;
    max-width: 45rem;
    margin: 0 auto;

    input {
      @include input-reset;
      width: 100%;
      color: black;
      background-color: white;
      font-size: 1rem;
      padding: 10px 16px;
      border-radius: 8px 0 0 8px;
      transition: 0.4s;

      &::placeholder {
        color: gray;
      }

      &:focus {
        box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
      }
    }

    button {
      @include input-reset;
      width: 100%;
      cursor: pointer;
      max-width: 4rem;
      background-color: #d3ba2d;
      padding: 7px 12px;
      border-radius: 0 8px 8px 0;
      color: #FFF;
      font-size: 1rem;
      text-transform: uppercase;
      transition: 0.4s;

      &:hover {
        background-color: #7f701c;
      }

      &:active {
        background-color: #3B8070;
      }
    }
  }

  // Grid Section
  .grid {
    @include grid(repeat(auto-fill, minmax(20rem, 2fr)), minmax(200px, auto), 20px, stretch, 2rem 5vw);

    article {
      border: 1px solid #ccc;
      box-shadow: 2px 2px 6px 0px rgba(0, 0, 0, 0.3);

      a {
        img {
          width: 100%;
          height: 30rem;
          object-fit: contain;
        }

        .text {
          padding: 20px;
          color: white;
        }
      }
    }
  }

  .svg-icon.search-icon {
    width: 1.3rem;
    height: 1.3rem;
    color: white;
  }

}
</style>