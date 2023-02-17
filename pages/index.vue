<template>
  <div class="home">
    <!-- hero -->
    <Hero />

    <!-- search -->
    <div class="container search">
      <input type="text" placeholder="Search" v-model.lazy="searchInput" />
      <button v-show="searchInput !== ''" @click="clearSearch" class="button">
        Clear search
      </button>
    </div>

    <!-- Loading -->
    <Loading v-if="$fetchState.pending"/>

    <!-- movies -->
    <div v-else class="container movies">
      <div id="movie-grid" class="movies-grid">
        <div class="movie" v-for="(movie, index) in movies" :key="index">
          <div class="movie-img">
            <img
              :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
              alt="movie banner"
            />
            <p class="review">{{ movie.vote_average }}</p>
            <p class="overview">{{ movie.overview }}</p>
          </div>
          <div class="info">
            <p>
              {{ movie.title.slice(0, 25) }}
              <span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString("en-us", {
                  month: "long",
                  day: "numeric",
                  year: "numeric",
                })
              }}
            </p>
            <nuxt-link
              class="button button-light"
              :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
            >
              Get More Info
            </nuxt-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Hero from "../components/hero.vue";
import Loading from "../components/loading.vue";

export default {
  name: "IndexPage",

  components: { Hero, Loading },

  data() {
    return {
      movies: [],
      searchInput: "",
      searchedMovie: []
    };
  },

  async fetch() {
    await this.getMovies();
  },

  methods: {
    async getMovies() {
      const data = this.$axios.get(
        "https://api.themoviedb.org/3/movie/now_playing?api_key=39c51a374a5355d1fded58f013170454&language=en-US&page=1"
      );

      const result = await data;
      this.movies = result.data.results;
    },

    clearSearch() {
      this.searchInput = "";
      this.searchedMovie = []
    },
  },
};
</script>

<style lang="scss" scoped>
.search {
  display: flex;
  padding: 32px 16px;
  input {
    max-width: 350px;
    width: 100%;
    padding: 10px 6px;
    font-size: 14px;
    border: 2px solid rgb(134, 134, 134);
    border-radius: 5px;
    &:focus {
      outline: none;
    }
  }
  .button {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
    margin-left: 0.2rem;
  }
}

.movies {
  padding: 32px 16px;
  .movies-grid {
    display: grid;
    column-gap: 32px;
    row-gap: 64px;
    grid-template-columns: 1fr;
    @media (min-width: 500px) {
      grid-template-columns: repeat(2, 1fr);
    }
    @media (min-width: 750px) {
      grid-template-columns: repeat(3, 1fr);
    }
    @media (min-width: 1100px) {
      grid-template-columns: repeat(4, 1fr);
    }
    .movie {
      position: relative;
      display: flex;
      flex-direction: column;
      .movie-img {
        position: relative;
        overflow: hidden;

        &:hover {
          .overview {
            transform: translateY(0);
          }
        }
        img {
          display: block;
          width: 100%;
          height: 100%;
        }
        .review {
          position: absolute;
          top: 0;
          left: 0;
          display: flex;
          justify-content: center;
          align-items: center;
          width: 40px;
          height: 40px;
          background-color: #c92502;
          color: #fff;
          border-radius: 0 0 16px 0;
          box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
            0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }
        .overview {
          line-height: 1.5;
          position: absolute;
          bottom: 0;
          background-color: rgba(201, 38, 2, 0.9);
          padding: 12px;
          color: #fff;
          transform: translateY(100%);
          transition: 0.3s ease-in-out all;
        }
      }
      .info {
        margin-top: auto;
        .title {
          margin-top: 8px;
          color: #fff;
          font-size: 20px;
        }
        .release {
          margin-top: 8px;
          color: #c9c9c9;
        }
        .button {
          margin-top: 8px;
          color: #000000;
        }
      }
    }
  }
}
</style>
