<template lang="html">
<v-form>
  <v-container fluid>
    <v-layout row wrap>

      <v-flex xs-12>
        <h2 class="display-3 font-weight-black">Search for users</h2>
      </v-flex>

      <!-- <App-search></App-search> -->
      <!-- <v-flex xs12>
       <v-text-field
         label="Type username"
         v-model="username"
         solo>
        </v-text-field>
      </v-flex> -->

      <v-flex xs-12 class="pa-3">
        <v-autocomplete
          v-model="user"
          :items="users"
          :loading="isLoading"
          :search-input.sync="search"
          color="white"
          hide-no-data
          hide-selected
          item-text="login"
          item-value="login"
          label="Public APIs"
          placeholder="Start typing to Search"
          return-object>
        </v-autocomplete>
      </v-flex>

      <v-flex xs-12>

        <v-layout row wrap v-if="user">
          <v-flex xs-4>
            <img :src="user.avatar_url" alt="">
            <h4>{{user.login}}</h4>
          </v-flex>
          <v-flex xs-8>
          </v-flex>
        </v-layout>

        <pre>
          {{user}}
        </pre>
      </v-flex>

    </v-layout>
  </v-container>
</v-form>
</template>

<script>
import axios from 'axios'
export default {
  // components: {
  //   AppSeach
  // },
  data: function () {
    return {
      users: [],
      user: null,
      isLoading: false,
      search: null
    }
  },
  watch: {
    search (val) {

      console.log('val', val, val.length);

      if(val.length > 4) {
        this.loadData(val);
      }

      // Items have already been requested
      if (this.isLoading) return

      this.isLoading = true

    }
  },
  methods: {
    loadData (val) {
      // Lazily load input items
      axios.get(`https://api.github.com/search/users?q=${this.search}`)
        .then(res => {
          console.log('data',res);
          const { count, entries } = res.data
          this.users = res.data.items
          // this.count = count
          // this.entries = entries
        })
        .catch(err => {
          console.log(err)
        })
        .finally(() => (this.isLoading = false))
    }
  }
}
</script>

<style lang="css">
body {
  font-family: 'Roboto'
}
</style>
