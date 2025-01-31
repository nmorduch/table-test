<script setup>
import { ref, computed } from 'vue'
import Country from './components/Country.vue'
import data from './assets/data.json'

const countries = ref([...data])
const sortDirection = ref('descending')
const sortColumn = ref('Total energy supply')
sortCountries()

const pageSize = 20
const totalPages = ref(Math.ceil(countries.value.length / pageSize))
const currentPage = ref(1)

const currentCountries = computed(() => {
  // Pages are indexed from 1 so subtract one for 0-indexed slice
  let pageStart = (currentPage.value - 1) * pageSize
  let pageEnd = currentPage.value * pageSize
  return countries.value.slice(pageStart, pageEnd)
})

function sortCountries(property = 'Total energy supply', direction = 'descending') {
  sortColumn.value = property
  sortDirection.value = direction
  countries.value.sort((a, b) => {
    let propertyA = a[property]
    let propertyB = b[property]
    if (propertyA == null) {
      return 1
    }
    if (propertyB == null) {
      return -1
    }
    if (sortDirection.value == 'ascending') {
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
    <button :disabled="currentPage == 1" @click="currentPage--">prev</button>
    Page {{ currentPage }} / {{ totalPages }}
    <button :disabled="currentPage == totalPages" @click="currentPage++">next</button>
    <table>
      <thead>
        <tr>
          <th scope="col" :aria-sort="sortColumn == 'country' ? sortDirection : null">
            Country
            <button @click="sortCountries('country', 'ascending')">asc</button>
            <button @click="sortCountries('country', 'descending')">desc</button>
          </th>
          <th scope="col" :aria-sort="sortColumn == 'region' ? sortDirection : null">
            Region
            <button @click="sortCountries('region', 'ascending')">asc</button>
            <button @click="sortCountries('region', 'descending')">desc</button>
          </th>
          <th scope="col" :aria-sort="sortColumn == 'Total energy supply' ? sortDirection : null">
            Total energy supply
            <button @click="sortCountries('Total energy supply', 'ascending')">asc</button>
            <button @click="sortCountries('Total energy supply', 'descending')">desc</button>
          </th>
          <th scope="col">Member</th>
        </tr>
      </thead>
      <tbody>
        <Country
          v-for="country in currentCountries"
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
