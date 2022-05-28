<template>
  <h1 id="label">Поиск Аниме</h1>
    <input type="text" v-model="anime">
    <button v-on:click="Searchanime"> Поиск </button>
  <div class="animess">
  <Animes v-for="anime of anime2" :key="anime.id"
  v-bind:anime="anime"/>
  </div>

</template>

<script>

//import {ref} from "vue";
import Animes from "@/components/Animes";

export default {
  name: 'App',
  data(){
    return{
      animelist: "null"
    }
  },
  components: {
    Animes
  },
  props : ['anime','animelist2'],
  methods:
      {
    async Searchanime(){
      //const animelist = ref([]);
      //console.log(`https://api.jikan.moe/v3/search/anime?q=${this.anime}`)
          this.animelist= await fetch(`https://api.jikan.moe/v3/search/anime?q=${this.anime}`)
              .then(res => res.json()).then(data => data.results)
      //console.log(this.animelist.value[0].image_url);
          console.log(this.animelist[0])
          //this.animelist2=animelist.value[0].image_url;
          //this.anime2=animelist.value[0];
          //console.log(this.anime2)
      }
    },
    computed:{
      // eslint-disable-next-line vue/return-in-computed-property
      anime2(){
        //const animelist = ref([]);
        // eslint-disable-next-line vue/no-async-in-computed-properties
        //animelist.value= fetch(`https://api.jikan.moe/v3/search/anime?q=${this.anime}`)
        //    .then(res => res.json()).then(data => data.results)
        //console.log(`https://api.jikan.moe/v3/search/anime?q=${this.anime}`)
        // eslint-disable-next-line vue/no-async-in-computed-properties
        //animelist.value = fetch(`https://api.jikan.moe/v3/search/anime?q=${this.anime}`)
        //    .then(res => res.json()).then(data => data.results)
        if (this.animelist!='null')
        return this.animelist
        else
          return this.anime2
      }
    }
    //watch: {
       //anime2(value) {

          //value
          //document.getElementById("name").innerText=value.title
         //console.log(value)
          //document.getElementById("description").innerText=value.synopsis
          //document.getElementById("ok").src=value.image_url
          //document.getElementById("date").innerText = value.start_date[0] + value.start_date[1] + value.start_date[2] + value.start_date[3] + "-" + value.end_date[0]+value.end_date[1]+value.end_date[2]+value.end_date[3];
          //document.getElementById("rate").innerText = "Рейтинг: "+value.score
          //document.getElementById("name")=value.
         //const animelist = ref([]);
         //console.log(`https://api.jikan.moe/v3/search/anime?q=${this.anime}`)
         //animelist.value= fetch(`https://api.jikan.moe/v3/search/anime?q=${this.anime}`)
             //.then(res => res.json()).then(data => data.results)
         //console.log(animelist.value[0].image_url);
         //this.animelist2=animelist.value[0].image_url;
         // eslint-disable-next-line no-unused-vars
         //value=animelist.value[0];
       // }
   // }

}
</script>

<style>
html{
  height: 100%;
  background: #4be061;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
body{
  margin: 0;
}
#label{
  margin-block-start: 0;
  margin-block-end: 0;
  font-size: 50px;
  margin: 20px;
}
input{
  font-size: 42px;
  font-weight: 400;
  margin-bottom: 30px;
}
button{
  font-size: 42px;
  margin-left: 10px;
}

</style>
