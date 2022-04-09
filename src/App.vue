<template>
    <input type="text" v-model="anime">
    <button v-on:click="Searchanime"> Hello </button>
    <div class="icon">
      <img id="ok"/>
    </div>
    <div class="information">
      <h1 id="name"></h1>
      <h2 id="date"></h2>
      <h3 id="description"></h3>
      <h4 id="rate"></h4>
    </div>
</template>

<script>

import {ref} from "vue";

export default {
  name: 'App',
  data(){
    return{
      anime2: "null"
    }
  },
  components: {
  },
  props : ['anime','animelist2'],
  methods:{
    async Searchanime(){
      const animelist = ref([]);
      //console.log(`https://api.jikan.moe/v3/search/anime?q=${this.anime}`)
          animelist.value= await fetch(`https://api.jikan.moe/v3/search/anime?q=${this.anime}`)
              .then(res => res.json()).then(data => data.results)
      console.log(animelist.value[0].image_url);
          //this.animelist2=animelist.value[0].image_url;
          this.anime2=animelist.value[0];
      }
    },
    watch: {
       anime2(value) {
          //value
          document.getElementById("name").innerText=value.title
         console.log(value)
          document.getElementById("description").innerText=value.synopsis
          document.getElementById("ok").src=value.image_url
          document.getElementById("date").innerText = value.start_date[0] + value.start_date[1] + value.start_date[2] + value.start_date[3] + "-" + value.end_date[0]+value.end_date[1]+value.end_date[2]+value.end_date[3];
          document.getElementById("rate").innerText = "Рейтинг: "+value.score
          //document.getElementById("name")=value.
        }
    }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
