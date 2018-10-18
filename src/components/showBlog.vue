<template>
  <div id="show-blog">
      <h1>All Blog Articles</h1>
      <div v-for="blog in blogs" class="single-blog">
          <h2>{{blog.title}}</h2>
          <article>{{blog.body}}</article>
      </div>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      blogs: [] //tworzymy pusta tablice do ktorej pobierzemy dane za pomoca GET request ze strony jason placeholder
    };
  },
  methods: {},
  created() {
    this.$http
      .get("https://jsonplaceholder.typicode.com/posts")
      .then(function(data) {
        console.log(data);
        this.blogs = data.body.slice(0, 10); //wybieramy tylko pierwsze 10 elementow z tablicy (ze strony json)
      });
  }
};
</script>

<style>
#show-blog {
    max-width: 800px;
    margin: 0 auto;
}
.single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background-color: greenyellow;
}
</style>
