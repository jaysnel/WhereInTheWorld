<template>
  <div class="flag-home-container">
      <div class="search-section">
            <b-form-input class="search-with-icon" placeholder="Search for a country..." v-model="searchKeyWords" @change="filterCountryOnSearchInput"></b-form-input>
            <input style="opacity: 0; z-index: -11111">
            <b-form-select v-model="regionalBlocSelected" :options="regionalBloc" @change="getCountriesByRegion"></b-form-select>
      </div>
      <div class="list-of-flags">
          <div v-for="item in allFlags" :key="item.numericCode">
              <b-card
                    v-bind:title="item.name"
                    v-bind:img-src="item.flag"
                    img-alt="Image"
                    img-top
                    tag="article"
                    style="max-width: 20rem;"
                    class="mb-2"
                    @click="getIndividualCountryInformation(item)"
                >
                    <b-card-text>
                    <p><span>Population:</span> {{ item.population }}</p>
                    <p><span>Region:</span> {{ item.region }}</p>
                    <p><span>Capital:</span> {{ item.capital }}</p>
                    </b-card-text>
                </b-card>
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
    name: "FlagHome",
    data() {
        return {
            originalList: [],
            allFlags: [],
            randomTenFlags: [],
            searchKeyWords: null,
            regionalBlocSelected: "All",
            regionalBloc: [{value: "All", text: "All"},{value: "Africa", text: "Africa"}, 
            {value: "Americas", text: "Americas"}, {value: "Asia", text: "Asia"}, {value: "Europe", text: "Europe"}, {value: "Oceania", text: "Oceania"},]
        }
    },
    watch: {
        searchKeyWords: function(e) {
            this.filterCountryOnSearchInput(e);
        }
    },
    mounted() {
        let page = this;
        page.getAllFlagsList();
    },
    methods: {
        getAllFlagsList() {
            let page = this;
            let url = 'https://restcountries.eu/rest/v2';

            axios.get(`${url}/all`)
            .then(res => {
                page.allFlags = res.data;
                page.originalList = res.data;
            })
            .catch(res => {
                console.log(res)
            })
        },
        filterCountryOnSearchInput(userSearchWords) {
            let page = this;
            let newList = page.originalList.filter(el => el.name.toLowerCase().includes(userSearchWords.toLowerCase()));

            page.allFlags = newList;
        },
        getCountriesByRegion() {
            let page = this;
            let url = "https://restcountries.eu/rest/v2/region";

            if(page.regionalBlocSelected === "All") {
                page.getAllFlagsList();
                return false;
            }

            axios.get(`${url}/${page.regionalBlocSelected}`)
            .then(res => {
                page.allFlags = res.data;
                page.originalList = res.data;
            })
        },
        getIndividualCountryInformation(country) {
            let page = this;
            let countryName = country.name;

            page.$router.push({ path: `/country/${countryName}`});
        }
    }
}
</script>

<style scoped>
    p {
        font-size: 15px;
        padding: 0;
        margin: 0;
    }

    input,
    select,
    .list-of-flags div .card {
        box-shadow: 0 4px 4px -2px rgba(0,0,0,.2);
    }

    .card:hover {
        cursor: pointer;
    }

    .search-with-icon::before {
        display: block;
        position: absolute;
        height: 500px;
        width: 500px;
        background: url("../assets/images/search.png");
    }

    .search-section {
        display: flex;
        flex-direction: space-between !important;
        margin: 15px 30px;
        padding: 30px 0;
    }

    .search-section input {
        width: 50%;
        margin-left: 12px;
    }

    .search-section select {
        width: 250px;
        margin-right: 12px;
    }

    .list-of-flags {
        display: flex;
        flex-wrap: wrap;
    }

    .list-of-flags div {
        text-align: left;
        margin: auto;
        width: 265px;
        margin-bottom: 70px;
    }

    .list-of-flags div img {
        max-width: 100%;
        width: 300px;
        height: 175px;
        margin: 0;
    }

    .list-of-flags div .card-body {
        margin: 0;
        margin-bottom: 20px;
        font-size: 16px;
    }

    .list-of-flags .card-title {
        font-size: 17px;
        font-weight: bold;
        margin-bottom: 20px;
    }

    .list-of-flags .card-text {
        text-align: left;
    }

    .list-of-flags .card-text span {
        font-weight: 600;
    }

    .list-of-flags div .card-body p {
        margin: 6px 0;
    }

    @media(min-width: 1000px) {
        .flag-home-container {
            width: 1440px;
            margin: auto;
        }
        .list-of-flags {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
        }
    }
    
</style>