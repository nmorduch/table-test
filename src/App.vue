<script setup>
import { ref } from 'vue'
import Country from './components/Country.vue'
import data from './assets/data.json'

const countries = ref([...data])
sortCountries()

function sortCountries(property = 'Total energy supply', ascending = false) {
  countries.value.sort((a, b) => {
    let propertyA = a[property]
    let propertyB = b[property]
    if (ascending) {
      if (propertyA < propertyB) {
        return -1
      }
      if (propertyA > propertyB) {
        return 1
      }
      return 0
    } else {
      if (propertyA < propertyB) {
        return 1
      }
      if (propertyA > propertyB) {
        return -1
      }
      return 0
    }
  })
}
</script>

<template>
  <header>
    <h1>Web Developer Test: Sortable Table</h1>
  </header>

  <main>
    <table>
      <thead>
        <tr>
          <th>
            Country
            <button @click="sortCountries('country', (ascending = true))">asc</button>
            <button @click="sortCountries('country', (ascending = false))">desc</button>
          </th>
          <th>
            Region
            <button @click="sortCountries('region', (ascending = true))">asc</button>
            <button @click="sortCountries('region', (ascending = false))">desc</button>
          </th>
          <th>
            Total energy supply
            <button @click="sortCountries('Total energy supply', (ascending = true))">asc</button>
            <button @click="sortCountries('Total energy supply', (ascending = false))">desc</button>
          </th>
          <th>Member</th>
        </tr>
      </thead>
      <tbody>
        <Country
          v-for="country in countries"
          :key="country.country"
          :countryName="country.country"
          :region="country.region"
          :totalEnergySupply="country['Total energy supply']"
          :member="country['member']"
        />
      </tbody>
    </table>
  </main>
</template>
