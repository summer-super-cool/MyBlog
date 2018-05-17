<template>
  <div v-theme:column="'narrow'"  id="show-blogs">
    <h1>博客总览</h1>
    <input type="text" v-model="search" placeholder="搜索">
    <div v-for="blog in filterBlogs" class="single-blog">
      <router-link v-bind:to="'/blog/' + blog.id">
        <h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
      </router-link>
       <article>
          {{blog.content | snippet}}
       </article>
    </div>
  </div>
</template>

<script>

export default {
  name: 'show-blogs',
  data(){
    return{
      blogs:[],
      search:""
    }
  },
  created(){
    this.$http.get('https://vuedemo-001.firebaseio.com/posts.json')
    .then(function(data){
      console.log(data.json());
      return data.json()
       // this.blogs = data.body.slice(0,10);
    })
      .then(function (data) {
        var blogsArray = [];
        for(let key in data){
          data[key].id = key;
          blogsArray.push(data[key]);
        }
        this.blogs = blogsArray;
      })
  },
  computed:{
    filterBlogs:function(){
       return this.blogs.filter((blog) =>{
          return blog.title.match(this.search);
       })
    }
  },//局部过滤器指令
  filters:{
//    "to-uppercase":function(value){
//        return value.toUpperCase();
//        }
//简写
      toUppercase(value){
          return value.toUpperCase();
          }
    },//局部自定义指令
    directives:{
       'rainbow':{
          bind(el,binding,vnode){
             el.style.color = "#" + Math.random().toString(16).slice(2,8);
          }
       }
    }

}
</script>

<style >
#show-blogs{
   max-width:800px;
   margin:0 auto;
}

#show-blogs a{
   color:#444;
   text-decoration:none;
}
.single-blog{
   padding:20px;
   margin:20px 0;
   background:#eee;
   border:1px dotted #aaa;
}
input[type="text"]{
   padding:8px;
   width:100%;
   box-sizing:border-box;
}

</style>
