<template>
  <div class="container">
    <h1>My Favorite Songs</h1>
    <div class="create-post">
      <div class="panel panel-success">
        <div class="panel-heading">Song Information</div>
        <div class="panel-body">
          <form>
            <div class="form-row form-group has-feedback" id="songform">
              <div class="col-md-3">
                <label for="song-name">Song Name</label>
              </div>
              <div class="col-md-9">              
                <input v-model="song" class='form-control' type='text' name='song-name' id='song-name' placeholder="Type Song Name..." />
              </div>
            </div>
            <div class="form-row form-group has-feedback" id="artistform">
              <div class="col-md-3">
                <label for="artist">Artist Name:</label>
              </div>
              <div class="col-md-9">
                <input v-model="artist" class='form-control' type='text' name='artist' id='artist' placeholder="Type Artist Name..." />
              </div>
            </div>
            <div class="form-row form-group has-feedback" id="Album">
              <div class="col-md-3">
                <label for="album">Album Name:</label>
              </div>
              <div class="col-md-9">
                <input v-model="album" class='form-control' type='text' name='album' id='album' placeholder="Type Album Name..." />
              </div>
            </div>
          </form>
        </div>
      </div>
      <button id="submit" class="btn btn-success" v-on:click="createPost">Post!</button>
      <h6>Double Click an Entry to Delete</h6>
    </div>
    <hr>
    <p class="error" v-if="error">{{ error }}</p>
    <div class="posts-container">
      <div class="post"
      v-for="(post, index) in posts"
      v-bind:item="post"
      v-bind:index="index"
      v-bind:key="post._id"
      v-on:dblclick="deletePost(post._id)"
      >
        <h1 class="display-4">Song Entry Information</h1>
        <p class="text">{{ `Song Name: ${post.song}`}}<br>{{`Artist Name: ${post.artist}`}}<br>{{`Album Name: ${post.album} `}}</p>
        {{ `Date Added to List: ${post.createdAt.getMonth() + 1}/${post.createdAt.getDate()}/${post.createdAt.getFullYear()}` }}
      </div>
    </div>
  </div>
</template>

<script>
import PostService from '../PostService';
import $ from "jquery";


export default {
  name: 'PostComponent',
  data() {
    return {
      posts: [],
      error: '',
      song: '',
      artist: '',
      album: '',
    }
  },
  async created() {
    try {
      this.posts = await PostService.getPosts();
    } catch(err) {
      this.error = err.message;
    }
  },
  methods: {
    async createPost() {
      await PostService.insertPost(this.song, this.artist, this.album);
      this.song = '';
      this.artist = '';
      this.album = '';
      $("#song-name").val('');
      $("#artist").val('');
      $("#album").val('');
      this.posts = await PostService.getPosts();
    },
    async deletePost(id) {
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div.container {
  max-width: 800px;
  margin: 0 auto;
}

#submit{
  width: 100%;
}

.panel > .panel-heading {
    background-image: none;
    background-color: #5bd658;
    color: white;

}

p.error {
  border: 1px solid #ff5b5f;
  background-color: #ffc5c1;
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

div.created-at{
  position: absolute;
  top: 0;
  left: 0;
  padding: 5px 15 px 5px 15px;
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
