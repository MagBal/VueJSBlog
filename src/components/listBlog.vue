<template>
  <div v-theme:column ="'narrow'" id="show-blog">
      <h1>List Blog Title</h1>
      <input type="text" v-model="search" placeholder="search blogs"/>
      <div v-for="blog in filteredBlog" class="single-blog">
          <h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
                </div>
    
  </div>
</template>

<script>
import searchMixin from '../mixins/searchMixin';

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
      .get("https://jsonplaceholder.typicode.com/posts")
      .then(function(data) {
        console.log(data);
        this.blogs = data.body.slice(0, 10); //wybieramy tylko pierwsze 10 elementow z tablicy (ze strony json)
      });
  },
  //dodanie metody typu computed - stworzenie search boxa do wyszukiwania blogow z okreslonymi znakami w tytule//zamiana tablicy blogs w dyrektywie v-for z template na filteredBlog
  computed: {
    
  },
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

<style>
#show-blogs{
    max-width: 800px;
    margin: 0px auto;
}
.single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
}
</style>