<template>
  <div id="post" class="post">
    <CreatePost class="createPost"/>
    <Post v-for="post in allPosts.reverse()" class="onepost" v-bind:key="post.id" :post="post"/>
  </div>
</template>

<script>
import axios from "axios";
import CreatePost from '@/components/CreatePost.vue';
import Post from '@/components/Post.vue';



export default {
  name: 'ListPostsView',
  components: {
    CreatePost,
    Post
  },
  data() {
    return {
      post: {
        id: "",
        message: "",
        imageUrl: "",
        userId:"",
        
      },
      allPosts: [],
      isAdmin: ""
    };
  },
  mounted() {
    var storage = JSON.parse(localStorage.getItem("user"));
    var token = storage.token;
    
    if (Storage === null) {
      this.$router.push("/");
      return;
    } 

    axios.get("http://localhost:3000/api/auth/profile/" + storage.userId,{
    headers: {Authorization: "Bearer " + token}})
      .then(response => {
        this.isAdmin  = response.data.isAdmin;
        if(this.isAdmin == true) {
          axios.get("http://localhost:3000/api/auth/post?isAdmin=true", {
          headers: {Authorization: "Bearer " + token}})
            .then(response => {
              this.allPosts = response.data;
            })
            .catch(error => { error
            })
        }
        else {
          axios.get("http://localhost:3000/api/auth/post", {
        headers: {
          Authorization: "Bearer " + token
        }
      })
      .then(response => {
        console.log("post", response.data);
        this.allPosts = response.data;
      })
      .catch(error => {
        console.log(error); //n'affiche pas le message 'normalement' envoyé par le back
      })
      //this.$store.dispatch("getUserInfos"); // problème pour recup userinfos
        }
      })    
  }  
};


</script>

<style scoped>
.post {
  min-height: 100%;
  margin-top: 50px;
}

.createPost {
  padding: 2px 100px 10px 100px;
  background: white;
  border-radius: 0.5rem;
  box-shadow: 0px 1px 8px 0px rgba(0, 0, 0, 0.06);
}

.onepost {
  margin-top: 20px;  
  box-shadow: 0px 1px 8px 0px rgba(0, 0, 0, 0.06);
  border-radius: 0.5rem;
  padding: 0.5rem;
  margin-bottom: 1rem;
  
}


</style>
