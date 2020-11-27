<template>
  <div>
    <Header />

    <Profile
      :user="user"
      :repos="repos"
      :starreds="starreds"
      v-if="user && repos.length && starreds.length"
    />

    <div class="container" v-else>
      <Loader />
    </div>
  </div>
</template>

<style lang="scss">
*:focus {
  @apply outline-none;
}
</style>

<script>
export default {
  components: {
    Header: () => import("@/components/layout/Header"),
    Profile: () => import("@/components/Profile"),
    Loader: () => import("@/components/layout/Loader"),
  },

  data() {
    return {
      user: null,
      repos: [],
      starreds: [],
    };
  },

  mounted() {
    this.handleUserInfo();
  },

  methods: {
    async handleUserInfo() {
      try {
        const user = this.getUserFromURL();

        this.user = await this.fetchUser(user);
        this.repos = await this.fetchRepos(user);
        this.starreds = await this.fetchStarreds(user);
      } catch (error) {
        console.log(error);
      }
    },

    async fetchUser(user) {
      user = user ? user : "ashkur";
      const userResponse = await fetch(`https://api.github.com/users/${user}`);
      return await userResponse.json();
    },

    async fetchRepos(user) {
      user = user ? user : "ashkur";
      const repoResponse = await fetch(
        `https://api.github.com/users/${user}/repos`
      );
      return await repoResponse.json();
    },

    async fetchStarreds(user) {
      user = user ? user : "ashkur";
      const starredResponse = await fetch(
        `https://api.github.com/users/${user}/starred`
      );
      return await starredResponse.json();
    },

    getUserFromURL() {
      let uri = window.location.search.substring(1);
      let params = new URLSearchParams(uri);

      return params.get("user");
    },
  },
};
</script>

<style>
</style>
