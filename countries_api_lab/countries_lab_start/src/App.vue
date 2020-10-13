<template>
  <div id="app">
    <h2>Global population: {{ globalPopulation }}</h2>

    <label for="country_select">Select a Country:</label>
    <select id="country_select" v-model="selectedCountry">
      <option disabled value="">Select a country</option>
      <option v-for="country in countries" :value="country">{{country.name}}</option>
    </select>

    <country-detail :country="selectedCountry"></country-detail>

    <button v-on:click="addFavourite">Add Country</button>

    <favourite-countries :favouriteCountries="favouriteCountries"></favourite-countries>
</div>

</template>

<script>
import CountryDetail from './components/CountryDetail.vue';
import FavouriteListItem from './components/FavouriteListItem.vue';

export default {
  name: 'App',
  data() {
    return {
      countries: [],
      selectedCountry: null,
      favouriteCountries: []
    }
  },
  components: {
    'country-detail': CountryDetail,
    'favourite-countries': FavouriteListItem
  },
    computed: {
      globalPopulation: function () {
        return this.countPopulation()
      }
    },
    mounted(){
      this.fetchCountries()
    },
    methods: {
      fetchCountries: function(){
        const request = fetch('https://restcountries.eu/rest/v2/all')
        .then(response => response.json())
        .then(data => this.countries = data)
    },
      addFavourite: function() {
        this.favouriteCountries.push(this.selectedCountry);
    },
    countPopulation: function () {
        return this.countries.reduce((population, country) => {
            return population += country.population;
        }, 0) 
      }
  },
}

</script>

<style>
.small-flag {
  height: 20px
}



</style>
