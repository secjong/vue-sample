<template>
  <div class="home">
    <b-button @click="axiosTest()">Axios Test</b-button>
    <b-table :items="itemList" v-if="hasResult"></b-table>
  </div>
</template>

<script>
// @ is an alias to /src
export default {
  name: 'home',
  data: function () {
    return {
      itemList: []
    }
  },
  computed: {
    hasResult: function () {
      return this.itemList.length > 0
    }
  },
  methods: {
    axiosTest: function(){
      axios.get("http://localhost:3000/movies/list/ajax", {})
      .then((response) => {
        console.log(response.data.movieList);
        this.itemList = response.data.movieList;
      })
      .catch(function(err){
        console.log(err);
      });
    }
  }
}
</script>
