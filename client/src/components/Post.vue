<template>
  <div class="hello">
    <div class="container">
      <h1>Latest Posts</h1>
      <div class="create-pos">
        <label for="create-post">Say Something...</label>
        <input v-model="text" type="text" id="create-post" />
        <button @click="createPost">Post</button>
      </div>
      <hr />
      <p v-if="error" class="error">{{ error }}</p>
      <div class="post-container">
        <div
          :item="post"
          :key="post._id"
          v-for="(post, index) in posts"
          :index="index"
          class="post"
        >
          <div class="created-at">
            {{
              `${post.createdAt.getDate()}/${post.createdAt.getMonth()}/${post.createdAt.getFullYear()}`
            }}
          </div>
          <p class="text">{{ post.text }}</p>
          <button @click="deletePost(post._id, index)">Delete</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PostService from "../postService";
export default {
  name: "Post",
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
    } catch (err) {
      this.error = err.message;
    }
  },
  methods: {
    async createPost() {
      await PostService.insertPost(this.text);
      this.posts = await PostService.getPosts();
      this.text = "";
    },

    async deletePost(id, index) {
      this.posts.splice(index, 1);
      await PostService.deletePost(id);
      
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
  border: 1px solid red;
  background-color: pink;
  padding: 10px;
  margin-bottom: 15px;
}
div.post {
  position: relative;
  border: 1px solid #5bd658;
  background-color: #bcffb8;
  padding: 10px 10px 30px 10px;
  margin-bottom: 15px;
}
div.created-at {
  position: absolute;
  top: 0;
  left: 0;
  padding: 5px 15px 5px 15px;
  background-color: darkgreen;
  color: white;
  font-size: 14px;
}
p.text {
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 0;
}
</style>
