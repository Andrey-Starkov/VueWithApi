<template>
  <h1 id="label">Search Anime</h1>
    <input type="text" v-model="anime">
    <button id="search" v-on:click="Searchanime"> Search </button>
    <div class="filters">
      <div class="filterr">
    <h3>Release Year filter</h3>
    <select v-model="filter">
      <option value="All">All</option>
      <option value="2022">2022</option>
      <option value="2021">2021</option>
      <option value="2020">2020</option>
      <option value="2019">2019</option>
      <option value="2018">2018</option>
      <option value="2017">2017</option>
      <option value="2016">2016</option>
      <option value="2015">2015</option>
      <option value="2014-2010">2014-2010</option>
      <option value="2009-2005">2009-2005</option>
      <option value="2004">2004 и ранее</option>
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

//import {ref} from "vue";
import Animes from "@/components/Animes";
import Loader from "@/components/Loader";
//import Loader from "@/components/Loader";
export default {
  name: 'App',
  data(){
    return{
      animelist: "null",
      filter: "All",
      aaa: "null",
      Sort: "null",
      Page: 0,
      Min_page:0,
      massive: "null",
      Max_page: "null",
      loading: false,

    }
  },
  components: {
    Loader,
    //Loader,
    Animes
  },
  props : ['anime','animelist2'],
  methods:
      {
    async Searchanime(){
          this.aaa= await fetch(`https://api.jikan.moe/v3/search/anime?q=${this.anime}`)
              .then(res => res.json()).then(data => data.results)
          this.Max_page=Math.floor(((this.aaa.filter(t => t.start_date !==null).length)-1)/6)
          this.massive=this.aaa.filter(t => t.start_date !==null)
          this.animelist=this.aaa.filter(t => t.start_date !==null).slice(this.Page*6,this.Page*6+6)
          this.Page=0
          this.loading=true
          setTimeout(()=>{
            this.loading=false
          },1000)
      },
    Pageright(){
      if (this.Page<this.Max_page)
      this.Page+=1
    },
        Pageleft(){
          if (this.Page>this.Min_page)
            this.Page-=1
        },
      Filters(s1){
          return this.animelist.filter(t => t.start_date.startsWith(s1))
      }
    },
  watch:{
    filter(){
      this.Page=0
      //this.Max_page=Math.floor(((this.animelist.length)-1)/6)
    },
    Page(){
      //console.log(this.aaa)
      //console.log(this.aaa.filter(t => t.start_date !==null).slice(this.Page*6,this.Page*6+6))
      if (this.anime2.length===0){
        this.Page-=1;
      }
      if (this.filter==="All") {
        //this.Max_page = Math.floor(((this.massive.length) - 1) / 6)
        this.animelist = this.massive.slice(this.Page * 6, this.Page * 6 + 6)
      }
      else
      if (this.filter==="2014-2010"){
        if (this.Sort==="left")
          this.animelist=this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2015 &&
              parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2009).sort((a,b)=>a.score>b.score ? 1 : -1).slice(this.Page * 6, this.Page * 6 + 6)
        else
        if (this.Sort==="right")
          this.animelist=this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2015 &&
              parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2009).sort((a,b)=>a.score>b.score ? -1 : 1).slice(this.Page * 6, this.Page * 6 + 6)
        else
          this.animelist=this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2015 &&
            parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2009).slice(this.Page * 6, this.Page * 6 + 6)
      }
      else
      if(this.filter==="2009-2005"){
        if (this.Sort==="left")
          this.animelist=this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2010 &&
              parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2004).sort((a,b)=>a.score>b.score ? 1 : -1).slice(this.Page * 6, this.Page * 6 + 6)
        else
        if (this.Sort==="right")
          this.animelist=this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2010 &&
              parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2004).sort((a,b)=>a.score>b.score ? -1 : 1).slice(this.Page * 6, this.Page * 6 + 6)
        else
        this.animelist=this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2010 &&
            parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2004).slice(this.Page * 6, this.Page * 6 + 6)
      }
      else
      if(this.filter==="2004"){
        if (this.Sort==="left")
          this.animelist=this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2005).sort((a,b)=>a.score>b.score ? 1 : -1).slice(this.Page * 6, this.Page * 6 + 6)
        else
        if (this.Sort==="right")
          this.animelist=this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2005).sort((a,b)=>a.score>b.score ? -1 : 1).slice(this.Page * 6, this.Page * 6 + 6)
        else
          this.animelist=this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2005).slice(this.Page * 6, this.Page * 6 + 6)
      }
      else {
        if (this.Sort==="left")
          this.animelist=this.massive.filter(t => t.start_date.startsWith(this.filter)).sort((a,b)=>a.score>b.score ? 1 : -1).slice(this.Page * 6, this.Page * 6 + 6)
        else
        if (this.Sort==="right")
          this.animelist=this.massive.filter(t => t.start_date.startsWith(this.filter)).sort((a,b)=>a.score>b.score ? -1 : 1).slice(this.Page * 6, this.Page * 6 + 6)
        else
        this.animelist=this.massive.filter(t => t.start_date.startsWith(this.filter)).slice(this.Page * 6, this.Page * 6 + 6)
      }

    }
   // filter(){}
  },
    computed: {
      // eslint-disable-next-line vue/no-dupe-keys
      // eslint-disable-next-line vue/return-in-computed-property
      anime2() {
        console.log(this.Sort)
        if (this.animelist !== 'null') {
          if (this.filter === "All") {
            if (this.Sort==="left"){

              // eslint-disable-next-line vue/no-side-effects-in-computed-properties
              return this.massive.sort((a,b)=>a.score>b.score ? 1 : -1).slice(this.Page * 6, this.Page * 6 + 6)
            }
            if (this.Sort==="right") {
              // eslint-disable-next-line vue/no-side-effects-in-computed-properties
              return this.massive.sort((a, b) => a.score > b.score ? -1 : 1).slice(this.Page * 6, this.Page * 6 + 6)
            }

            return this.massive.slice(this.Page * 6, this.Page * 6 + 6)
          }

          if (this.filter === "2014-2010") {
            if (this.Sort==="left")
              return this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2015 &&
                  parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2009).sort((a,b)=>a.score>b.score ? 1 : -1).slice(this.Page * 6, this.Page * 6 + 6)

            if (this.Sort==="right")
              return this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2015 &&
                  parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2009).sort((a,b)=>a.score>b.score ? -1 : 1).slice(this.Page * 6, this.Page * 6 + 6)

            return this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2015 &&
               parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2009).sort((a,b)=>a.score>b.score ? -1 : 1).slice(this.Page * 6, this.Page * 6 + 6)
          }
          if (this.filter === "2009-2005") {
            if (this.Sort==="left")
              return this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2010 &&
                  parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2004).sort((a,b)=>a.score>b.score ? 1 : -1).slice(this.Page * 6, this.Page * 6 + 6)

            if (this.Sort==="right")
              return this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2010 &&
                  parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2004).sort((a,b)=>a.score>b.score ? -1 : 1).slice(this.Page * 6, this.Page * 6 + 6)

            return this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2010 &&
                parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) > 2004).slice(this.Page * 6, this.Page * 6 + 6)
          }

          if (this.filter === "2004") {
            if (this.Sort==="left")
              return this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2005).sort((a,b)=>a.score>b.score ? 1 : -1).slice(this.Page * 6, this.Page * 6 + 6)

            if (this.Sort==="right")
              return this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2005).sort((a,b)=>a.score>b.score ? -1 : 1).slice(this.Page * 6, this.Page * 6 + 6)

            return this.massive.filter(t => parseInt(t.start_date[0] + t.start_date[1] + t.start_date[2] + t.start_date[3]) < 2005).slice(this.Page * 6, this.Page * 6 + 6)
          }

          if(this.filter!=null) {
            if (this.Sort === "left")
              return this.massive.filter(t => t.start_date.startsWith(this.filter)).sort((a, b) => a.score > b.score ? 1 : -1).slice(this.Page * 6, this.Page * 6 + 6)

            if (this.Sort === "right")
              return this.massive.filter(t => t.start_date.startsWith(this.filter)).sort((a, b) => a.score > b.score ? -1 : 1).slice(this.Page * 6, this.Page * 6 + 6)

            //console.log(this.massive.filter(t => t.start_date.startsWith(this.filter)).slice(this.Page * 6, this.Page * 6 + 6))
            return this.massive.filter(t => t.start_date.startsWith(this.filter)).slice(this.Page * 6, this.Page * 6 + 6)
          }

        } else
          return this.anime2
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
  margin: 0px 5px 5px;
}
</style>
