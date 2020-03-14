<template>
  <div class="virus">
      <h1 class="title">COVID-19</h1>
      <AutocompleteDropdown :options="options" placeholder="Country" @input="updateCountry"/>
      <p class="value">Confirmed: {{ confirmed }}</p>
      <p class="value">Dead: {{ dead }}</p>
      <p class="value">Recovered: {{ recovered }}</p>
  </div>
</template>

<script>
import AutocompleteDropdown from './AutocompleteDropdown'

export default {
    name: 'VirusStats',
    components: {
        AutocompleteDropdown
    },

    data () {
        return {
            options: [],

            country: '',
            confirmed: 0,
            recovered: 0,
            dead: 0
        }
    },

    methods: {
        async getCounties () {
            const data = await fetch('https://covid19.mathdro.id/api/countries')
            const json = await data.json()
            const countries = []
            for (const country in json.countries) {
                countries.push(country)
            }

            this.options = countries
        },

        async updateCountry (country) {
            if (country === this.country) return

            this.country = country
            this.dead = 0
            this.confirmed = 0
            this.recovered = 0

            this.getInfected(country)
        },

        async getInfected (country) {
            try {
                const data = await fetch(`https://covid19.mathdro.id/api/countries/${country}`)
                const json = await data.json()

                this.dead = json.deaths.value
                this.confirmed = json.confirmed.value
                this.recovered = json.recovered.value
            } catch (error) {
                this.dead = 0
                this.confirmed = 0
                this.recovered = 0
            }
        }
    },

    created () {
        this.getCounties()
    }
}
</script>

<style scoped lang="scss">
.title {
    font-size: 1.7em;
}

.value {
    font-size: 1.1em;
    font-weight: bold;
}
</style>
