<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-2 mb-5">
        <button class="btn btn-success" @click="promptName">
          Ingresar Datos
        </button>
      </div>
    </div>
    <div class="row justify-content-center" v-if="loggedIn">
      <div class="col-6 mb-5">
        <div class="card sombra">
          <div class="card-body">
            <h1 class="card-title">Bienvenid@ {{ fullName }}</h1>
            <p class="card-text text-body-secondary">
              Aquí podrás encontrar información de películas de anime de Studio
              Ghibli
            </p>
            <button class="btn btn-warning" @click="clearName">
              Limpiar información
            </button>
          </div>
        </div>
      </div>
      <div class="alert alert-info mb-4">
        <p class="text-center fw-bold">
          A continuación hay un listado con los nombres de las películas
        </p>
      </div>
      <select
        v-model="selectedMovie"
        class="form-select mb-3"
        aria-label="Default select example"
        @change="setMovie"
      >
        <option selected disabled>Selecciona una película</option>
        <option :value="index" v-for="(movie, index) in movies" :key="index">
          {{ movie.title }}
        </option>
      </select>

      <div class="col-6 mb-5" v-if="movieSelected">
        <div class="card sombra">
          <img
            :src="moviePoster"
            class="card-img-top"
            alt="Poster de la película"
          />
          <div class="card-body">
            <h3 class="card-title">
              <b>Título Original:</b> {{ movieHepburn }}
            </h3>
            <h5><b>Título en inglés:</b> {{ movieTitle }}</h5>
            <br />
            <p><b>Descripción:</b> {{ movieSynopsis }}</p>
            <br />
            <h5><b>Director:</b> {{ movieDirector }}</h5>
            <h6 class="text-body-secondary">
              <b>Duración:</b> {{ movieRuntimeMinutes }} minutos
            </h6>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ghibli-get",
  // props: {},
  data: function () {
    return {
      name: "",
      lastname: "",
      loggedIn: false,
      //MOVIE STUFF
      moviePoster: "",
      movieTitle: "",
      movieHepburn: "",
      movieSynopsis: "",
      movieDirector: "",
      movieRuntimeMinutes: "",
      movieSelected: false,
      movies: [],
      selectedMovie: "",
      start: true,
    };
  },
  computed: {
    fullName() {
      return this.name + " " + this.lastname;
    },
  },
  methods: {
    promptName() {
      this.name = prompt("Ingrese su nombre");
      this.lastname = prompt("Ingrese su Apellido");
      this.loggedIn = true;
      if (this.start) {
        this.getMovies();
      }
    },
    clearName() {
      this.name = "";
      this.lastname = "";
      this.movieSelected = false;
      this.loggedIn = false;
    },
    setMovie() {
      let i = this.selectedMovie;
      let movie = this.movies[i];
      this.moviePoster = movie.poster;
      this.movieTitle = movie.title;
      this.movieHepburn = movie.hepburn;
      this.movieSynopsis = movie.synopsis;
      this.movieDirector = movie.director;
      this.movieRuntimeMinutes = movie.runtimeMinutes;
      this.movieSelected = true;
    },
    async getMovies() {
      const axios = require("axios");
      try {
        const response = await axios.get(
          "https://studio-ghibli-films-api.herokuapp.com/api/"
        );
        this.movies = Object.values(response.data);
        this.start = false;
      } catch (error) {
        console.error(error);
      }
    },
  },
};
// watch: {},
// components: {},
// mixins: [],
// filters: {},
// -- Lifecycle Methods
// -- End Lifecycle Methods
</script>

<style scoped>
.sombra {
  box-shadow: rgba(238, 90, 231, 0.24) 0px 3px 8px;
}
</style>