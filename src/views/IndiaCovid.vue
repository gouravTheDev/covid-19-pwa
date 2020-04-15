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
          <router-link class="link" to="/india">
            <div>
              <b-card
                tag="article"
                style="max-width: 20rem;"
                class="h-100 mx-auto bcard shadow mb-2 rounded"
              >
                <b-card-text class="text-left">
                  Total Case
                  <h4>{{ indiaTotal }}</h4>
                  Conf. Indian
                  <h4>{{ confirmedCasesIndian }}</h4>
                  Conf. Foreigner
                  <h4>{{ confirmedCasesForeign }}</h4>
                  Discharged
                  <h4>{{ indianDischarged }}</h4>
                  Deaths
                  <h4>{{ indianDeaths }}</h4>
                </b-card-text>
              </b-card>
            </div>
          </router-link>
        </div>
      </div>
    </div>
    <br />
    <p>
      <i>
        Source:-
        <a href="https://www.mygov.in/covid-19/?cbps=1">COVID-19 | MyGov.in</a>
      </i>
    </p>
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
      <b-button size="sm" class="mBtn" type="submit" @click="searchState"
        >Search</b-button
      >
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
                <h5 class="font-weight-bold">{{ state.loc }}</h5>
                <b-icon icon="plus-circle-fill" variant="danger"></b-icon>&nbsp;
                <span class="font-weight-bold">Total Case:-</span>
                {{ state.totalConfirmed }}
                <br />
                <b-icon icon="plus-circle-fill" variant="warning"></b-icon
                >&nbsp;
                <span class="font-weight-bold">Conf. Cases:-</span>
                {{ state.totalConfirmed }}
                <br />
                <b-icon icon="plus-circle-fill" variant="success"></b-icon
                >&nbsp;
                <span class="font-weight-bold">Recovered:-</span>
                {{ state.discharged }}
                <br />
                <b-icon icon="plus-circle-fill" variant="danger"></b-icon>&nbsp;
                <span class="font-weight-bold">Deaths:-</span>
                {{ state.deaths }}
              </b-card-text>
              <!-- <b-card-text class="text-left">
                <h5 class="font-weight-bold">{{state.loc}}</h5>

                <span class="font-weight-bold">Total Case:-</span>
                <h4>{{state.totalConfirmed}}</h4>

                <span class="font-weight-bold">Conf. Cases:-</span>
                <h4>{{state.totalConfirmed}}</h4>

                <span class="font-weight-bold">Recovered:-</span>
                <h4>{{state.discharged}}</h4>

                <span class="font-weight-bold">Deaths:-</span>
                <h4>{{state.deaths}}</h4>
              </b-card-text>-->
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
      pageTitle: 'Indian Statistics',
      indiaTotal: null,
      confirmedCasesIndian: null,
      confirmedCasesForeign: null,
      indianDischarged: null,
      indianDeaths: null,
      statewiseDetails: [],
      tempStates: null,
      stateName: null,
      isLoading: false,
      fullPage: true
      // api:'http://api.go/API/ParentApp/V1/'
    }
  },
  components: {
    Loading
  },
  computed: mapState('app', ['appTitle']),
  created() {
    this.showLoader()
    this.fetchIndiaDetails()
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
            // console.log(data.data.regional)
            self.statewiseDetails = data.data.regional
            self.tempStates = data.data.regional
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
    searchState() {
      this.tempStates = this.statewiseDetails.filter(state => {
        return state.loc.toLowerCase().includes(this.stateName.toLowerCase())
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
  background-color: rgb(19, 3, 63);
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
