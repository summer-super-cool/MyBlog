<template>
   <div id="single-blog">
     <p>{{blog.title}}</p>
     <p>博客内容:</p>
     <p>{{blog.content}}</p>
     <p>博客分类:</p>
     <ul>
       <li v-for="category in blog.categories">
         {{category}}
       </li>
     </ul>
     <p>作者：</p>
     <p>{{blog.author}}</p>

     <button><router-link :to="'/edit/' + id">编辑</router-link></button>
     <button @click="deleteSingleBlog()">删除</button>
   </div>

</template>

<script>
  export default{
     name:"single-blog",
     data(){
        return{
            id:this.$route.params.id,
            blog:{}
        }
     },
     created(){
        this.$http.get('https://vuedemo-001.firebaseio.com/posts/'
           + this.id + ".json")
           .then(function(data){
             return data.json();
             console.log(data);
            this.blog = data.body;
           })
          .then(function(data){
              this.blog = data;
       })
     },
     methods:{
       deleteSingleBlog(){
         this.$http.delete("https://vuedemo-001.firebaseio.com/posts/" + this.id + ".json")
           .then(response => {
             this.$router.push({path:'/'})
           })
       }
     }
  }

</script>
<style scoped>
#single-blog{
   max-width:960px;
   margin:0 auto;
   padding:20px;
   background:#eee;
   border:1px dotted #eee;
}
button{
  display:block;
  margin:20px 10px;
  background:crimson;
  color:#fff;
  border:0;
  padding:14px 40px;
  border-radius:4px;
  font-size:18px;
  cursor:pointer;
  float: left;
}
button a{
  text-decoration: none;
  color: #fff;
}
</style>