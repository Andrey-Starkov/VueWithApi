<template>
  <div class="all">
    <div class="icon">
      <img id="ok" :src="anime.images.jpg.image_url" />
    </div>
    <div class="information">
      <h1 id="name">{{anime.title}}</h1>
      <h3 id="description">{{description}}</h3>
      <h2 id="rate">Rating myanimelist: {{anime.score}}</h2>
      <h4 id="date">{{anime.aired.string}}</h4>
      <a :href="anime.url">Link on anime</a>
    </div>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Animes",
  props: ["anime"],
  computed: {
    description() {
      if(this.anime.synopsis.length>256)
        return this.anime.synopsis.slice(0,255) + "..."
      return this.anime.synopsis
    }
  }
}
</script>

<style scoped>
.all {
  display: inline-block;
  width: 25%;
  max-width: 25%;
  height: 40%;
  max-height: 40%;
  margin-right: 100px;
  border: 4px solid black;
  background: #fc3;
  margin-bottom: 20px;
}
img{
  margin-top: 5px;
  height: 300px;
  max-width: 350px;
}
#description{
  width: 75%;
  margin: auto;
  height: 200px;
}
#name{
  height: 180px;
}
</style>
