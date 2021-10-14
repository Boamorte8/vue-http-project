<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="error">{{ error }}</p>
      <p v-else-if="results.length === 0">No stored experiences found: Start adding some surveys results</p>
      <ul v-else>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  // props: ['results'],
  data() {
    return {
      results: [],
      isLoading: false,
      error: null,
    }
  },
  components: {
    SurveyResult,
  },
  methods: {
    // async loadExperiences() {
    //   const response = await fetch('https://vue-http-survey-default-rtdb.firebaseio.com/surveys.json');
    //   console.log(response);
    // },
    loadExperiences() {
      this.isLoading = true;
      this.error = null;
      fetch('https://vue-http-survey-default-rtdb.firebaseio.com/surveys.json')
        .then(function(response) {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          const results = [];
          for (const id in data) {
            results.push({
              id,
              ...data[id],
            })
          }
          this.results = results;
          this.isLoading = false;
        })
        .catch(error => {
          console.log(error);
          this.error = 'Failed to fetch data - please try again later.';
          this.isLoading = false;
        });
    },
  },
  mounted() {
    this.loadExperiences();
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>