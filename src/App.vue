<template>
  <div id="container">
    <div id="app">
      <header>
        <h1>Twitter User Search Tool</h1>
      </header>
      <body>
        <div id="content">
          <h2>Enter Twitter User ID</h2>
          <div id="dataEntry">
            <input type="text" v-model="twitterId" maxlength="50" />
          </div>
          <button class="btn btn-primary" v-on:click="getData()">
            Get User Data
          </button>
          <div v-if="display">
            <br />
            <img :src="imageUrl" alt="Twitter Profile Pic" />
            <p>{{ twitterId }}</p>
            <p>{{ userId }}</p>
            <p>{{ location }}</p>
            <p>{{ description }}</p>
            <div v-if="pinnedTweetId">
              <p>Pinned Tweet Id: {{ pinnedTweetId }}</p>
              <p>Pinned Tweet Text: {{ pinnedTweetText }}</p>
            </div>
          </div>
        </div>
      </body>
    </div>
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";
import axios from "axios";

export default {
  name: "app",
  // components: {
  //   HelloWorld
  // },
  data: function() {
    return {
      twitterId: "",
      userId: "",
      name: "",
      location: "",
      description: "",
      imageUrl: "",
      createdDate: "",
      pinnedTweetId: "",
      pinnedTweetText: "",
      display: false
    };
  },
  methods: {
    getData: function() {
      this.pinnedTweetText = "";
      this.pinnedTweetId = "";
      axios
        .get("https://cryptic-cove-28362.herokuapp.com/" + this.twitterId)
        // .get("http://localhost:5000/" + this.twitterId)
        .then(response => {
          this.userId = response.data.data.id;
          this.name = response.data.data.name;
          this.location = response.data.data.location;
          this.createdDate = response.data.data.created_at;
          this.description = response.data.data.description;
          this.imageUrl = response.data.data.profile_image_url;
          if (response.data.includes) {
            this.pinnedTweetId = response.data.includes.tweets[0].id;
            this.pinnedTweetText = response.data.includes.tweets[0].text;
          }
          // eslint-disable-next-line no-console
          console.log(this.imageUrl);
          this.display = true;
        })
        .catch(error => {
          this.errors = error;
        });
    }
  }
};
</script>

<style scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100vw;
  height: 100vh;
}
header,
body {
  padding-top: 15px;
  background-color: lightskyblue;
}

input[type="text"] {
  width: 375px;
}

#dataEntry {
  padding-bottom: 15px;
}

#container {
  background-color: lightskyblue;
}
</style>
