<template>
  <div class="icon">
    <img id="ok" src="{{anime.image_url}}" />
  </div>
  <div class="information">
    <h1 id="name">{{anime.title}}</h1>
    <h2 id="date">{{anime.start_date[0]}}{{anime.start_date[1]}}{{anime.start_date[2]}}{{anime.start_date[3]}}</h2>
    <h3 id="description">{{anime.synopsis}}</h3>
    <h4 id="rate">{{anime.score}}</h4>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Animes",
  props: ["anime"]
}
</script>

<style scoped>

</style>
