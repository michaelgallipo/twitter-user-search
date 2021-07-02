<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "app",
  components: {
    HelloWorld
  },
  data: function() {
    return {
      twitterId: "",
      userId: "",
      name: "",
      pinned_tweet_id: "",
      pinned_tweet_text: "",
      display: false
    }
  },
  methods: {
    getData: function() {
      const token = process.env.BEARER_TOKEN;
      axios
      .get("https://api.twitter.com/2/users/by?usernames=" + this.twitterId + 
      "&expansions=pinned_tweet_id", {
        headers: {
          'User-Agent': "v2UserLookupJS",
          'Authorization': `Bearer ${token}`
  }
})
      .then(response => {
        // this.tutorData = response.data;
        console.log(this.response);
      })
      .catch(error => {
        this.errors = error.response.data.errors;
      });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
