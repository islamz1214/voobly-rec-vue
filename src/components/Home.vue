<template>

  <b-container>
     <h2 id="title">1v1 Voobly Expert AOM Titan Recordings</h2>

<div>
  <b-navbar toggleable="lg">

      <b-navbar-nav>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="mx-auto">
        <b-nav-form>
          <b-nav-item>Total recs:  {{ totalFiles }}</b-nav-item>
          <b-nav-item>Created by: LNC_Trust</b-nav-item>
        <b-nav-item href="https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=islamz1214%40gmail.com&currency_code=USD">Donate</b-nav-item>
        </b-nav-form>

      </b-navbar-nav>

  </b-navbar>

   <b-navbar toggleable="lg">

      <b-navbar-nav>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="mx-auto">
        <b-nav-form>
          <b-form-input id="aom-search" type="text" v-model="search" placeholder="search ex. hades loki"/>

        </b-nav-form>

      </b-navbar-nav>
  </b-navbar>

</div>

  <hr>

    <b-row>
      <a
        v-for="file in filteredFiles"
        :key="file"
        :href="'/aomrecs/' + file"
        target="_blank"

        download>{{ file }}</a>
    </b-row>
   <!-- <b-pagination
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      first-text="First"
      prev-text="Prev"
      next-text="Next"
      last-text="Last"
      align="center"
      @input="paginate(currentPage)">
    </b-pagination>-->
  </b-container>

</template>

<script>
import axios from 'axios'

var url = process.env.VUE_APP_SERVER_PATH

export default {
  name: 'aomrcx',
  props: {
    msg: String
  },
  data () {
    return {
      files: [],
      displayFiles: [],
      search: ''
    }
  },
  mounted () {
    axios
      .get(url)
      .then((response) => {
        this.files = response.data
        this.displayFiles = response.data.slice(0, 10)
        this.rows = this.files.length
      })
  },
  computed: {

    filteredFiles: function () {
      // This allows users to filter the search with multiple words that are seprated by an empty space
      var multiSearch = this.search.replace(/^\s+|\s+$/g, '').replace(/\s+/g, ' ').split(' ')

      function multiFilter (search, files) {
        if (search.length === 0) {
          return files
        } else {
          files = files.filter(file => file.toLowerCase().match(search[0].toLowerCase()))
          search.shift()
          return multiFilter(search, files)
        }
      }

      return multiFilter(multiSearch, this.files)
    },

    totalFiles: function () {
      return this.files.length
    }
  }

}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
