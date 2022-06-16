<template>
  <h1 id="label">Search Anime</h1>
  <input type="text" v-model="anime">
  <button id="search" v-on:click="Searchanime"> Search </button>
  <div class="filters">
    <div class="filterr">
      <h3>Release Year filter</h3>
      <select v-model="filter">
        <option value="All">All</option>
        <option value="2000">до 2000</option>
        <option value="2010">до 2010</option>
        <option value="2015">до 2015</option>
        <option value="2020">до 2020</option>
      </select>
    </div>
    <div class="Sort">
      <h3>Rating</h3>
      <input type="radio" name="sort" id="BigSort" value="left" v-model="Sort">
      <label for="BigSort">по возрастанию</label>
      <input type="radio" name="sort" id="SmallSort" value="right" v-model="Sort">
      <label for="SmallSort">по убыванию</label>
    </div>
    <div class="page_menu">
      <h3 id="pages">Pages</h3>
      <h5>6 anime on one page</h5>
      <button class="page_button" v-on:click="Pageleft"> &lt; </button>
      <label>{{Page+1}}</label>
      <button class="page_button" v-on:click="Pageright"> > </button>
    </div>
  </div>
  <div class="animess">
    <Loader v-if="loading"></Loader>
    <Animes v-else v-for="anime of anime2" :key="anime.id"
            v-bind:anime="anime"/>
  </div>

</template>

<script>
import Animes from "@/components/Animes";
import Loader from "@/components/Loader";
export default {
  name: 'App',
  data(){
    return{
      animelist: "null",
      filter: "All",
      aaa: "null",
      Page: 0,
      Min_page:0,
      Sort:"null",
      loading: false,
    }
  },
  components: {
    Loader,
    Animes
  },
  props : ['anime'],
  methods:
      {
        async Searchanime(){
          this.animelist= (await fetch(`https://api.jikan.moe/v4/anime?q=${this.anime}`).then(res => res.json())).data
          this.loading=true
          setTimeout(()=>{
            this.loading=false
          },1000)
        },
        Pageright(){
          if (this.Page<this.Maxpage)
            this.Page+=1
        },
        Pageleft(){
          if (this.Page>this.Min_page)
            this.Page-=1
        },
      },
  watch:{

    async filter() {
      if (this.filter!=="All"){
        this.Page=0
        this.animelist = (await fetch(`https://api.jikan.moe/v4/anime?q=${this.anime}&start_date=${this.filter}`).then(res => res.json())).data
      }
      else {
        this.Page=0
        this.animelist = (await fetch(`https://api.jikan.moe/v4/anime?q=${this.anime}`).then(res => res.json())).data
      }

      if (this.Sort==="left")
        this.animelist = this.animelist.sort((a,b)=>a.score>b.score ? 1 : -1)

      if (this.Sort === "right")
        this.animelist = this.animelist.sort((a,b)=>a.score>b.score ? -1 : 1)

    },
    async Sort(){
        if (this.Sort==="left"){
          // По идеи сортировка должна работать так,как строчкой ниже, но почему то апи не хочет выдавать нужные результаты
          //this.animelist = (await fetch(`https://api.jikan.moe/v4/anime?q=${this.anime}&start_date=${this.filter}&order_by=score&sort=asc`).then(res => res.json())).data
          this.animelist = this.animelist.sort((a,b)=>a.score>b.score ? 1 : -1)
        }
        if (this.Sort === "right"){
          //this.animelist = (await fetch(`https://api.jikan.moe/v4/anime?q=${this.anime}&start_date=${this.filter}&order_by=score&sort=desc`).then(res => res.json())).data

          this.animelist = this.animelist.sort((a,b)=>a.score>b.score ? -1 : 1)
        }
    }
  },
  computed: {
    // eslint-disable-next-line vue/no-dupe-keys
    // eslint-disable-next-line vue/return-in-computed-property
    anime2() {
      console.log(this.Sort)
      if (this.animelist !== 'null') {
        return this.animelist.slice(this.Page * 6, this.Page * 6 + 6)
      } else
        return this.anime2
    },

    Maxpage(){
      return Math.floor((this.animelist.length-1)/6)
    }


  }
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
#search{
  font-size: 42px;
  margin-left: 10px;
}
select{
  font-size: 20px;
  margin-bottom: 1vw;
}
.filters{
  display: flex;
  align-items: center;
  text-align: center;
  justify-content: space-between;
  width: 50%;
  margin-left: 27vw;
}
#pages{
  margin-block-end: 0;
  margin-block-start: 0
}
h5{
  margin-block-start: 0;
  margin-block-end: 0.5em;
}
.page_button{
  margin: 0 5px 5px;
}
</style>
