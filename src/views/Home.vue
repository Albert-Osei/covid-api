<template>
  <div class="home">
    <div class="header">
      <h2>Covid Updates Around the World</h2>
    </div>
    <div v-if="info && info.length !=0" class="container">
      <div v-for="(detail, index) in details" :key="index" class="flashcards">
        <h2  v-if="detail.All.country !=0"> Country: {{ detail.All.country }}</h2>
        <h2 v-else>Country: Unspecified</h2>
        <p> Population: {{ detail.All.population }}</p>
        <p> Life expectancy: {{ detail.All.life_expectancy }}</p>
        <p> Confirmed cases: {{ detail.All.confirmed }}</p>
        <p> Number of deaths: {{ detail.All.deaths }}</p>
        <p> Recovered cases: {{ detail.All.recovered }}</p>
        <p> Last updated: {{ detail.All.updated | formatDate}}</p>
        <a href="https://covid19.who.int/" target="_blank">
          <small class="forward">Find out more</small>
        </a>    
      </div>
    </div>
    <div v-else>
      Please wait, the data is loading..
    </div>

    
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
import moment from 'moment'


export default {
  name: 'Home',
  data() {
    return {
      info: null,
      loading: false,
      details: null,
    };
  },
  components: {},
  mounted() {
    this.getApiData();
  },
  filters: {
    formatDate(val) {
      return moment(val).format("dddd, MMMM Do YYYY, h:mm:ss a")
    }
  },
  methods: {
    getApiData() {
      this.loading = true;
      axios.get("https://covid-api.mmediagroup.fr/v1/cases")
      .then((res) => {
        this.info = res;
        this.details = res.data
        console.log(res);
      })
      .catch((error) => {
        console.log(error);
        this.error = true;
      })
      .finally((this.loading = false));
    },
  },
}
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Quicksand:wght@300;400;600;700&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,700;1,400&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-column-gap: 20px;
  grid-row-gap: 10px;
  padding: 50px;
}

.flashcards {
  border: none;
  box-shadow: 0px 5px 15px rgba(160, 163, 184, 0.75);
  padding: 1rem;
  transition: 0.2s;
  cursor: pointer;
}

.flashcards:hover {
  background: #e1e9ecc4;
}

.flashcards > h2 {
  font-family: Quicksand;
  font-weight: bold;
  font-size: 24px;
  color: #8c99cc;
}

.flashcards p {
  font-family: Quicksand;
  font-weight: 600;
  font-size: 18px;
  color: rgb(158, 114, 136);
}

.forward {
  font-family: Roboto;
  font-weight: 400;
  font-size: 18px;
}

.header {
  height: 70px;
  background: #3d4f9f;
  box-shadow: 0px 5px 15px #6480e8;
}

.header > h2 {
  font-family: Quicksand;
  font-weight: 400;
  font-size: 20px;
  color: #efeff7;
  padding: 20px;
  text-align: center;
}
</style>
