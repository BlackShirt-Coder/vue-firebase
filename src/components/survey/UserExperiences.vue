<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadData">Load Submitted Experiences</base-button>
      </div>
      <p v-if="loading">Loading....</p>
      <p style="color:red" v-else-if="results.length === 0 && !loading && errorMessage === null">There is no data!!!</p>
      <p style="color:red" v-else-if="errorMessage !== null && !loading || !result">{{ errorMessage }}</p>


      <ul v-if="!loading && errorMessage === null">
        <survey-result v-for="result in results" :key="result.id" :name="result.name"
          :rating="result.rating"></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {

  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      loading: false,
      errorMessage: null
    }
  },
  mounted() {
    this.loadData();
  }
  ,
  methods: {
    loadData() {
      this.loading = true
      fetch('https://my-test-bed66-default-rtdb.firebaseio.com/surveys.json')
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          this.loading = false
          for (let i in data) {
            this.results.unshift({
              id: data[i].id,
              name: data[i].name,
              rating: data[i].rating
            })
          }
        })
        .catch(() => {
          this.loading = false
          this.errorMessage = 'Failed To Fetch Data - Please Try Again!!';
        })

        ;

    }
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>