<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">

        <!-- errors -->
        <div class="error" v-if="error">
          <p>{{ error }}</p>
        </div>

        <search
        :value="search"
        placeholde="Type username..."
        @search="search = $event"/>
        <button v-if="!repos" class="btn btnPrimary" @click="getRepos">Search</button>
        <button v-else class="btn btnPrimary" @click="getRepos">Search Again</button>

        <div class="repos__wrapper" v-if="repos">
          <div class="repos-item" v-for="repo in repos" :key="repo.id">
            <div class="repos-info">
              <a class="link" :href="repo.html_url" target="_blank">{{ repo.name }}</a>
              <span class="repos-span">{{ repo.stargazers_count }} <span class="stars">⭐</span></span>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import search  from "@/components/Search.vue";
import axios from "axios";
export default {
  components: {search},
  data () {
    return {
      search: '',
      repos: null,
      error: null
    }
  },
  methods: {
    getRepos () {
      axios
      .get(`https://api.github.com/users/${this.search}/repos`)
      .then(res => {
        this.error = null
        this.repos = res.data
      })
      .catch(err => {
        this.repos = null
        this.error = 'Cant find this user'
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.container{
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
button {
  margin-top: 40px;
}
.repos__wrapper{
  width: 400px;
  margin: 30px 0;
}
.repos-info {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 10px 0;
  border-bottom: 1px solid #dbdbdb;
}
.stars{
  padding-left: 5px;
}
.error{
  margin-block: 20px;
  color: red;
}
</style>