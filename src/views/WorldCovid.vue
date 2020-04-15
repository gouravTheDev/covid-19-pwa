<template>
  <div class="text-center">
    <loading
      :active.sync="isLoading"
      :can-cancel="false"
      :is-full-page="fullPage"
      :opacity="0.8"
    ></loading>
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
                  Total Case
                  <h4>{{ worldDetails.cases }}</h4>
                  New Cases
                  <h4>{{ worldDetails.todayCases }}</h4>
                  Deaths
                  <h4>{{ worldDetails.deaths }}</h4>
                  New Deaths
                  <h4>{{ worldDetails.todayDeaths }}</h4>
                  Critical
                  <h4>{{ worldDetails.critical }}</h4>
                  Recovered
                  <h4>{{ worldDetails.recovered }}</h4>
                  Active
                  <h4>{{ worldDetails.active }}</h4>
                </b-card-text>
              </b-card>
            </div>
          </router-link>
        </div>
      </div>
      <br />
      <p>
        <i>
          Source:-
          <a href="https://www.worldometers.info/coronavirus"
            >www.worldometers.info/coronavirus</a
          >
        </i>
      </p>
    </div>
    <hr />

    <h3 class="home-page-title">Country Wise Statistics</h3>
    <div class="col-lg-5 col-sm-12 mx-auto">
      <b-form-input
        size="sm"
        class="mr-sm-2"
        @keyup="searchCountry"
        placeholder="Search your country"
        v-model="countryName"
      ></b-form-input>
      <b-button size="sm" class="mBtn" type="submit" @click="searchCountry"
        >Search</b-button
      >
    </div>
    <br />
    <div class="col-lg-12 col-md-12 col-sm-12 mx-auto">
      <div class="row mx-auto">
        <div
          class="col-lg-3 col-md-6 col-sm-12"
          v-for="country in countryWiseDetails"
          v-if="country.country != '' && country.country != 'World'"
        >
          <div>
            <b-card
              tag="article"
              style="max-width: 20rem;"
              class="h-100 mx-auto shadow mb-2 rounded"
            >
              <b-card-text class="text-left">
                <h6 class="text-center font-weight-bold">
                  {{ country.country }}
                </h6>
                <b-icon icon="plus-circle-fill" variant="dark"></b-icon>&nbsp;
                <span class="font-weight-bold">Total Cases:-</span>
                {{ country.cases }}
                <br />
                <b-icon icon="plus-circle-fill" variant="warning"></b-icon
                >&nbsp;
                <span class="font-weight-bold">New Cases:-</span>
                {{ country.todayCases }}
                <br />
                <b-icon icon="plus-circle-fill" variant="danger"></b-icon>&nbsp;
                <span class="font-weight-bold">Deaths:-</span>
                {{ country.deaths }}
                <br />
                <b-icon icon="plus-circle-fill" variant="danger"></b-icon>&nbsp;
                <span class="font-weight-bold">New Deaths:-</span>
                {{ country.todayDeaths }}
                <br />
                <b-icon icon="plus-circle-fill" variant="warning"></b-icon
                >&nbsp;
                <span class="font-weight-bold">Critical:-</span>
                {{ country.critical }}
                <br />
                <b-icon icon="plus-circle-fill" variant="success"></b-icon
                >&nbsp;
                <span class="font-weight-bold">Recovered:-</span>
                {{ country.recovered }}
                <br />
                <b-icon icon="plus-circle-fill" variant="primary"></b-icon
                >&nbsp;
                <span class="font-weight-bold">Active:-</span>
                {{ country.active }}
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
import Loading from 'vue-loading-overlay'
// Import stylesheet
import 'vue-loading-overlay/dist/vue-loading.css'

export default {
  data() {
    return {
      pageTitle: 'GLobal Statistics',
      worldDetails: [],
      countryWiseDetails: null,
      tempCountries: null,
      countryName: null,
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
    this.fetchGlobalDetails()
    this.fetchAllCountries()
  },
  methods: {
    fetchGlobalDetails() {
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
            // console.log(data)
            self.countryWiseDetails = data
            self.tempCountries = data
          })
        })
        .catch(function(err) {
          console.log('Fetch Error :-S', err)
        })
    },
    searchCountry() {
      this.countryWiseDetails = this.tempCountries.filter(country => {
        return country.country
          .toLowerCase()
          .includes(this.countryName.toLowerCase())
      })
    },
    showLoader() {
      this.isLoading = true
      // simulate AJAX
      setTimeout(() => {
        this.isLoading = false
      }, 3500)
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
