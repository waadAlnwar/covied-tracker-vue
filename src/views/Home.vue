<template>
  <main v-if="!loading">
    <data-title :text='title' :timestamp='dataDate'/>

    <data-boxes :status='status'/>

    <country-select :countries='countries' @get-country='getCountryData'/>

    <button v-show="status.Country" @click="clearCountry" class=' bg-green-700 text-white rounded p-3 mt-6 mb-10 focus:outline-none hover:bg-green-600'>clear country</button>
  </main>

  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      fetching data
    </div>
    <img :src="loadingImg" class="w-24 m-auto">
  </main>
    
</template>

<script>
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes.vue'
import CountrySelect from '@/components/CountrySelect.vue'

export default {
  name: 'Home',
  components: {DataTitle, DataBoxes, CountrySelect, },

  
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate:'',
      status: {},
      countries: [],
      loadingImg: require('../assets/Coronavirus.gif')
    }
  },


  async created() {
    const data = await this.covidData()
    this.dataDate = data.Date
    this.status = data.Global
    this.countries = data.Countries
    this.loading = false
  },


  methods: {
    async covidData(){
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },

    getCountryData(country){
      this.status = country
      this.title = country.Country

    },

    async clearCountry(){
      this.loading = true
      const data = await this.covidData()
      this.title = 'Global'
      this.status = data.Global
      this.loading = false
    }
  },
}
</script>
