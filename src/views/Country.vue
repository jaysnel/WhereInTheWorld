<template>
    <div>
        <div class="country-container">
        <div class="flag">
            <b-button class="back-button" @click="goBackToHomePage">Back</b-button>
            <img v-bind:src="countryFlag" alt="">
        </div>
        <div class="country-info">
            <div v-for="item in countryInfo" :key="item">
                <h1>{{ item.name }}</h1>
                <div class="country-info-text">
                    <div>
                        <p><span>Native Name:</span> {{ item.nativeName }}</p>
                        <p><span>Population:</span> {{ item.population }}</p>
                        <p><span>Region:</span> {{ item.region }}</p>
                        <p><span>Sub Region:</span> {{ item.subregion }}</p>
                        <p><span>Capital:</span> {{ item.capital }}</p>
                    </div>
                    <div>
                        <p><span>Top Level Domain:</span> {{ item.topLevelDomain[0] }}</p>
                        <p v-for="item in item.currencies" :key="item"><span>Currencies:</span> {{ item.code }}</p>
                        <div class="multi-list"><span>Languages: </span><p v-for="item in item.languages" :key="item"> {{ item.name }},</p></div>
                    </div>
                </div>

                <div class="multi-list"><span>Border Countries: </span><p class="border-buttons" v-for="item in item.borders" :key="item">{{ item }}</p></div>
            </div>
        </div>
    </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: "Country",
    data() {
        return {
            countryInfo: [],
            countryFlag: null
        }
    },
    mounted() {
        let page = this;
        page.getCountryInformation();
    },
    methods: {
        getCountryInformation() {
            let page = this;
            let url = "https://restcountries.eu/rest/v2/name";
            let countryName = page.$route.params.country;
            
            axios.get(`${url}/${countryName}`)
            .then(res => {
                page.countryInfo = res.data;
                page.countryFlag = res.data[0].flag;
                console.log(res.data[0], page.countryFlag);
            })
            .catch(res => {
                console.log(res)
            })
        },
        goBackToHomePage() {
            let page = this;
            page.$router.push({ path: '/'});
        }
    }
}
</script>

<style scoped>
h1 {
    font-weight: 800;
}

.country-container {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    align-items: center;
    justify-items: center;
    height: 100vh;
}

.country-container .flag {
    display: grid;
}

.back-button {
    border-radius: 5px;
    width: 100px;
    margin-left: 15px;
    margin-bottom: 15px;
}

.country-container .flag img {
    max-width: 100%;
    width: 550px;
    border-radius: 5px;
}

.country-info {
    text-align: left;
}

.country-info span {
    font-weight: 800;
}

.country-info-text {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    margin: 0;
    padding: 0;
}

.multi-list {
    display: flex;
    flex-direction: row;
}

.border-buttons {
    background: transparent;
    box-shadow: 0 1px 3px #222;
    border-radius: 2px;
    padding: 0 20px;
    margin: 0 10px;
    text-align: center;
    font-size: 14px;
}

</style>