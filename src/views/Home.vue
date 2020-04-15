<template>
  <div class="text-center">
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
                  <b-icon icon="plus-circle-fill" variant="danger"></b-icon>
                  Total Case:-
                  {{indiaTotal}}
                  <br />
                  <b-icon icon="plus-circle-fill" variant="warning"></b-icon>
                  Conf. Indian:-
                  {{confirmedCasesIndian}}
                  <br />
                  <b-icon icon="plus-circle-fill" variant="warning"></b-icon>
                  Conf. Foreigner:-
                  {{confirmedCasesForeign}}
                  <br />
                  <b-icon icon="plus-circle-fill" variant="success"></b-icon>
                  Discharged:-
                  {{indianDischarged}}
                  <br />
                  <b-icon icon="plus-circle-fill" variant="danger"></b-icon>
                  Deaths:-
                  {{indianDeaths}}
                  <br />
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
          <router-link class="link" to="/india">
            <div>
              <b-card
                title="Global Statistics"
                tag="article"
                style="max-width: 20rem;"
                class="h-100 mx-auto bcard shadow rounded"
              >
                <b-card-text class="text-left">
                  <b-icon icon="plus-circle-fill" variant="danger"></b-icon>
                  Total Case:-
                  {{worldTotal}}
                  <br />
                  <b-icon icon="plus-circle-fill" variant="success"></b-icon>
                  Recovered:-
                  {{worldRecovered}}
                  <br />
                  <b-icon icon="plus-circle-fill" variant="danger"></b-icon>
                  Deaths:-
                  {{worldDeaths}}
                  <br />
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
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'

export default {
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
      indiaTotal: null,
      confirmedCasesIndian: null,
      confirmedCasesForeign: null,
      indianDischarged: null,
      indianDeaths: null,
      worldTotal: null,
      worldDeaths: null,
      worldRecovered: null
      // api:'http://api.go/API/ParentApp/V1/'
    }
  },
  computed: mapState('app', ['appTitle']),
  created() {
    this.fetchIndiaDetails()
    this.fetchWorldDetails()
  },
  methods: {
    fetchIndiaDetails() {
      // console.log(process.env.VUE_APP_TITLE);
      var self = this
      fetch('https://api.rootnet.in/covid19-in/stats/latest')
        .then(function(response) {
          if (response.status !== 200) {
            console.log(
              'Looks like there was a problem. Status Code: ' + response.status
            )
            return
          }
          response.json().then(function(data) {
            console.log(data.data.summary)
            var indianOb = data.data.summary
            self.indiaTotal = indianOb.total
            self.confirmedCasesIndian = indianOb.confirmedCasesIndian
            self.confirmedCasesForeign = indianOb.confirmedCasesForeign
            self.indianDischarged = indianOb.discharged
            self.indianDeaths = indianOb.deaths
          })
        })
        .catch(function(err) {
          console.log('Fetch Error :-S', err)
        })
    },
    fetchWorldDetails() {
      // console.log(process.env.VUE_APP_TITLE);
      var self = this
      fetch('https://coronavirus-19-api.herokuapp.com/all')
        .then(function(response) {
          if (response.status !== 200) {
            console.log(
              'Looks like there was a problem. Status Code: ' + response.status
            )
            return
          }
          response.json().then(function(data) {
            console.log(data)
            var worldOb = data
            self.worldTotal = worldOb.cases
            self.worldRecovered = worldOb.recovered
            self.worldDeaths = worldOb.deaths
          })
        })
        .catch(function(err) {
          console.log('Fetch Error :-S', err)
        })
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
