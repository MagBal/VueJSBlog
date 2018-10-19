<template>
  <div v-theme:column ="'narrow'" id="show-blog">
      <h1>All Blog Articles</h1>
      <input type="text" v-model="search" placeholder="search blogs"/>
      <div v-for="blog in filteredBlog" class="single-blog">
          <router-link v-bind:to="'/blog/' + blog.id"> <h2 v-rainbow>{{blog.title | to-uppercase}}</h2></router-link>
          <article>{{blog.content | snippet}}</article>
      </div>
    
  </div>
</template>

<script>
import searchMixin from "../mixins/searchMixin";

export default {
  data() {
    return {
      //tworzymy pusta tablice do ktorej pobierzemy dane za pomoca GET request ze strony jason placeholder
      blogs: [],
      //custom search filter - do wyszukiwania okreslonego bloga
      search: ""
    };
  },
  methods: {},
  created() {
    this.$http
      .get("https://nn-vue-blog-2e775.firebaseio.com/posts.json")
      .then(function(data) {
        return data.json()
        // this.blogs = data.body.slice(0, 10); //wybieramy tylko pierwsze 10 elementow z tablicy (ze strony json)
      }).then(function(data){
        var blogsArray = [];
        for(let key in data){
          data[key].id = key;
          blogsArray.push(data[key]);
        }
        // console.log(blogsArray);
        this.blogs = blogsArray;
      })
  },
  //dodanie metody typu computed - stworzenie search boxa do wyszukiwania blogow z okreslonymi znakami w tytule//zamiana tablicy blogs w dyrektywie v-for z template na filteredBlog
  computed: {},
  //filter locally
  filters: {
    toUppercase(value) {
      return value.toUpperCase();
    }
  },
  //directives locally
  directives: {
    rainbow: {
      bind(el, binding, vnode) {
        el.style.color =
          "#" +
          Math.random()
            .toString()
            .slice(2, 8);
      }
    }
  },
  mixins: [searchMixin]
};
</script>

<style scoped>
#show-blog{
    max-width: 800px;
    margin: 0px auto;
}
.single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
}
#show-blog a{
    color: #444;
    text-decoration: none;
}
input[type="text"]{
    padding: 8px;
    width: 100%;
    box-sizing: border-box;
}
</style>