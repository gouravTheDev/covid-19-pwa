<template>
  <div class="text-center">
    <loading :active.sync="isLoading" :can-cancel="false" :is-full-page="fullPage" :opacity="0.8"></loading>
    <h1 class="home-page-title">{{ appTitle }}</h1>

    <div class="col-lg-7 col-md-7 col-sm-12 mx-auto">
      <div class="row mx-auto">
        <div class="col-lg-6 col-md-12 col-sm-12">
          <router-link class="link" to="/india">
            <div>
              <b-card
                title="Indian Statistics"
                tag="article"
                style="max-width: 20rem;"
                class="h-100 mx-auto bcard shadow mb-2 rounded"
              >
                <b-card-text class="text-left">
                  Total Case
                  <h4>{{ indiaDetails.cases }}</h4>New Cases
                  <h4>{{ indiaDetails.todayCases }}</h4>Deaths
                  <h4>{{ indiaDetails.deaths }}</h4>New Deaths
                  <h4>{{ indiaDetails.todayDeaths }}</h4>Critical
                  <h4>{{ indiaDetails.critical }}</h4>Recovered
                  <h4>{{ indiaDetails.recovered }}</h4>Active
                  <h4>{{ indiaDetails.active }}</h4>
                  <br />
                  <p class="text-center text-muted small">
                    <i>Tap to know more</i>
                  </p>
                </b-card-text>
              </b-card>
            </div>
          </router-link>
        </div>
        <div class="col-lg-6 col-md-12 col-sm-12">
          <router-link class="link" to="/world">
            <div>
              <b-card
                title="Global Statistics"
                tag="article"
                style="max-width: 20rem;"
                class="h-100 mx-auto bcard shadow rounded"
              >
                <b-card-text class="text-left">
                  Total Case
                  <h4>{{ worldDetails.cases }}</h4>New Cases
                  <h4>{{ worldDetails.todayCases }}</h4>Deaths
                  <h4>{{ worldDetails.deaths }}</h4>New Deaths
                  <h4>{{ worldDetails.todayDeaths }}</h4>Critical
                  <h4>{{ worldDetails.critical }}</h4>Recovered
                  <h4>{{ worldDetails.recovered }}</h4>Active
                  <h4>{{ worldDetails.active }}</h4>
                  <br />
                  <p class="text-center text-muted small">
                    <i>Tap to know more</i>
                  </p>
                </b-card-text>
              </b-card>
            </div>
          </router-link>
        </div>
      </div>
      <br />
      <br />
      <p>
        <i>
          Source:-
          <a
            href="https://www.worldometers.info/coronavirus"
          >www.worldometers.info/coronavirus</a>
        </i>
      </p>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import Loading from 'vue-loading-overlay'
// Import stylesheet
import 'vue-loading-overlay/dist/vue-loading.css'

export default {
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
      indiaDetails: null,
      worldDetails: null,
      isLoading: false,
      fullPage: true
    }
  },
  components: {
    Loading
  },
  computed: mapState('app', ['appTitle']),
  created() {
    this.showLoader()
    this.fetchIndiaDetails()
    this.fetchWorldDetails()
  },
  methods: {
    fetchIndiaDetails() {
      // console.log(process.env.VUE_APP_TITLE);
      var self = this
      fetch('https://coronavirus-19-api.herokuapp.com/countries/India')
        .then(function(response) {
          if (response.status !== 200) {
            console.log(
              'Looks like there was a problem. Status Code: ' + response.status
            )
            return
          }
          response.json().then(function(data) {
            console.log(data)
            self.indiaDetails = data
          })
        })
        .catch(function(err) {
          console.log('Fetch Error :-S', err)
        })
    },
    fetchWorldDetails() {
      // console.log(process.env.VUE_APP_TITLE);
      var self = this
      fetch('https://coronavirus-19-api.herokuapp.com/countries/World')
        .then(function(response) {
          if (response.status !== 200) {
            console.log(
              'Looks like there was a problem. Status Code: ' + response.status
            )
            return
          }
          response.json().then(function(data) {
            console.log(data)
            self.worldDetails = data
          })
        })
        .catch(function(err) {
          console.log('Fetch Error :-S', err)
        })
    },
    showLoader() {
      this.isLoading = true
      // simulate AJAX
      setTimeout(() => {
        this.isLoading = false
      }, 2000)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/theme/variables.scss';

.page-wrapper {
  display: flex;
  flex-direction: column;
  // justify-content: center;
  align-items: center;

  .documentation-link {
    display: inline-block;
    font-size: 1.2rem;
    color: #fff;
    background-color: #5d6788;
    padding: 0.8rem 1.6rem;
    border-radius: 4px;
    transition: background-color 0.1s ease;
    box-sizing: border-box;
    text-decoration: none;
    width: fit-content;
    font-weight: 500;
  }
}
.bcard {
  width: 100%;
  min-height: 18rem;
  background-color: #eaf0f1;
}
.logo {
  margin-bottom: 3rem;
}

.home-page-title {
  text-align: center;
}
.link {
  text-decoration: none;
  color: black;
}
</style>
