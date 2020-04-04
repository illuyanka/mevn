<template>
  <div class="container">
    <h1>Latest Posts</h1>
    <div class="create-post form-group">
      <div class="form-group">
        <label for="create-post">Say something... (Double click for deleting) </label>
        <input class="form-control" type="text" id="create-post" v-model="text" placeholder="Create a post" />
      </div>
      <div class="form-control-group">
        <button class="btn btn-primary btn-block" v-on:click="createPost">Post!</button>
      </div>
    </div>
    <hr />
    <p class="error" v-if="error">{{error}}</p>
    <div class="post-container">
      <div
        class="post"
        v-for="(post, index) in posts"
        v-bind:item="post"
        v-bind:index="index"
        v-bind:key="post._id"
        v-on:dblclick="deletePost(post._id)"
      >
        {{ `${post.createdAt.getDate()}/${post.createdAt.getMonth()+1}/${post.createdAt.getFullYear()}`}}
        <p class="text">{{post.text}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import PostService from "../PostService";

import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
export default {
  name: "PostComponent",
  data() {
    return {
      posts: [],
      error: "",
      text: ""
    };
  },
  async created() {
    try {
      this.posts = await PostService.getPosts();
    } catch (error) {
      this.error = error.message;
    }
  },
  methods: {
    async createPost() {
      await PostService.insertPost(this.text);
      this.posts = await PostService.getPosts();
    },
     async deletePost(id) {
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div.container {
  max-width: 800px;
  margin: 0 auto;
}

p.error {
  border: 1px solid #ff5b5f;
  background-color: #ffc5c1;
  padding: 10px;
  margin-bottom: 15px;
}

div.post {
  position: relative;
  background-color: #35495e;
  padding: 10px 10px 30px 10px;
  margin-bottom: 15px;
  color:#fff;
}
.btn-primary {
    color: #fff;
    background-color: #41b883!important;
    border-color: #41b883!important;
}
div.created-at {
  position: absolute;
  top: 0;
  left: 0;
  padding: 5px 15px 5px 15px;
  background-color: darkgreen;
  color: white;
  font-size: 13px;
}

p.text {
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 0;
}
</style>