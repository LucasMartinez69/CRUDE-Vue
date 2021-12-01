<template>
  <div>
      <h1>Lista de pelis</h1>
      <ul>
          <li v-for="(movie, index) in movies" :key="index">
            {{ index }} - {{ movie.title }}
            <button @click="removeMovie(index)">Delete</button>
            <button @click="editMovie(index)">Edit</button>
          </li>
      </ul>
      <input type="text" v-model="newMovie.title"/>
      <button v-if="mode == 'add'" @click="addMovie">Add</button>
      <button v-if="mode == 'edit'" @click="updateMovie">Update</button>
      <p>{{ newMovie.index }} - {{ newMovie.title }}</p>
  </div>
</template>

<script>

import axios from "axios";

export default {
    data(){
        return {
            indexToUpdate:"",
            mode: "add",
            newMovie: {},
            movies: [],
        };
    },

    mounted(){
        this.fetchAll();
    },

    methods: {
        addMovie(){
            //this.movies.push(this.newMovie);
            if (!this.newMovie.title) return;
            this.movies = [...this.movies, this.newMovie];
            this.newMovie = {};
        },
        removeMovie(index){
            this.movies.splice(index, 1)
        },
        editMovie(index){
            this.newMovie = this.movies[index];
            this.indexToUpdate = index;
            this.mode = "edit";
        },
        updateMovie(){
            this.movies[this.indexToUpdate] = this.newMovie;
            this.newMovie = {};
            this.mode = "add";
        },
        fetchAll(){
            axios.get('http://localhost:3000/peliculas')
                 .then(res => (this.movies = res.data))
        }
    },      
};
</script>

<style scoped>
ul{
    list-style: none;
} 
</style>>

</style>