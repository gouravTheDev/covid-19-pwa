<template>
  <div class="text-center">
    <h1 class="home-page-title">{{ pageTitle }}</h1>

    <div class="col-lg-6 col-md-6 col-sm-12 mx-auto">
      <div class="row mx-auto">
        <div class="col-lg-12 col-md-12 col-sm-12">
          <router-link class="link" to="/world">
            <div>
              <b-card
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
                </b-card-text>
              </b-card>
            </div>
          </router-link>
        </div>
      </div>
    </div>
    <hr />

    <h3 class="home-page-title">State Wise Statistics</h3>
    <div class="col-lg-5 col-sm-12 mx-auto">
      <b-form-input
        size="sm"
        class="mr-sm-2"
        @keyup="searchState"
        placeholder="Search your state"
        v-model="stateName"
      ></b-form-input>
      <b-button size="sm" class="mBtn" type="submit" @click="searchState">Search</b-button>
    </div>
    <br />
    <div class="col-lg-12 col-md-12 col-sm-12 mx-auto">
      <div class="row mx-auto">
        <div class="col-lg-3 col-md-6 col-sm-12" v-for="state in tempStates">
          <div>
            <b-card
              tag="article"
              style="max-width: 20rem;"
              class="h-100 mx-auto shadow mb-2 rounded"
            >
              <b-card-text class="text-left">
                <h6 class="text-center font-weight-bold">{{state.loc}}</h6>
                <b-icon icon="plus-circle-fill" variant="danger"></b-icon>
                <span class="font-weight-bold">Total Case:-</span>
                {{state.totalConfirmed}}
                <br />
                <b-icon icon="plus-circle-fill" variant="warning"></b-icon>
                <span class="font-weight-bold">Conf. Cases:-</span>
                {{state.totalConfirmed}}
                <br />
                <b-icon icon="plus-circle-fill" variant="success"></b-icon>
                <span class="font-weight-bold">Recovered:-</span>
                {{state.discharged}}
                <br />
                <b-icon icon="plus-circle-fill" variant="danger"></b-icon>
                <span class="font-weight-bold">Deaths:-</span>
                {{state.deaths}}
              </b-card-text>
            </b-card>
          </div>
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
      pageTitle: 'GLobal Statistics',
      worldTotal: null,
      worldDeaths: null,
      worldRecovered: null,
      countryWiseDetails: [],
      tempStates: null,
      stateName: null
      // api:'http://api.go/API/ParentApp/V1/'
    }
  },
  computed: mapState('app', ['appTitle']),
  created() {
    this.fetchGlobalDetails()
    this.fetchAllCountries()
  },
  methods: {
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
    },
    fetchAllCountries() {
      // console.log(process.env.VUE_APP_TITLE);
      var self = this
      fetch('https://coronavirus-19-api.herokuapp.com/countries')
        .then(function(response) {
          if (response.status !== 200) {
            console.log(
              'Looks like there was a problem. Status Code: ' + response.status
            )
            return
          }
          response.json().then(function(data) {
            console.log(data)
            // self.statewiseDetails = data.data.regional
            // self.tempStates = data.data.regional
            // var indianOb = data.data.summary
            // self.indiaTotal = indianOb.total
            // self.confirmedCasesIndian = indianOb.confirmedCasesIndian
            // self.confirmedCasesForeign = indianOb.confirmedCasesForeign
            // self.indianDischarged = indianOb.discharged
            // self.indianDeaths = indianOb.deaths
          })
        })
        .catch(function(err) {
          console.log('Fetch Error :-S', err)
        })
    },
    searchState() {
      this.tempStates = this.statewiseDetails.filter(state => {
        return state.loc.toLowerCase().includes(this.stateName.toLowerCase())
      })
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/theme/variables.scss';

@media only screen and (max-width: 600px) {
  .mBtn {
    margin-top: 10px;
    display: inline-block;
  }
}
@media only screen and (min-width: 602px) {
  .mBtn {
    display: none;
  }
}
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
  color: #fff;
  background-color: rgb(7, 77, 10);
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
