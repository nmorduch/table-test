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
  countries.value.forEach((el, index) => {
    el.id = index + 1
  })
}
</script>

<template>
  <header>
    <h1>Web Developer Test: Sortable Table</h1>
  </header>

  <main>
    <table :aria-rowcount="countries.length">
      <caption>
        Total energy supply of countries
        <span class="sr-only">
          (column headers with buttons are sortable; pagination buttons are below the table).
        </span>
      </caption>
      <thead>
        <tr>
          <th
            class="table__col-wide"
            scope="col"
            :aria-sort="sortColumn == 'country' ? sortDirection : null"
          >
            Country
            <div class="table__sort-button-group">
              <button
                class="table__sort-button"
                @click="sortCountries('country', 'ascending')"
                aria-label="Sort ascending"
              >
                ▲
              </button>
              <button
                class="table__sort-button"
                @click="sortCountries('country', 'descending')"
                aria-label="Sort descending"
              >
                ▼
              </button>
            </div>
          </th>
          <th
            class="table__col-wide"
            scope="col"
            :aria-sort="sortColumn == 'region' ? sortDirection : null"
          >
            Region
            <div class="table__sort-button-group">
              <button
                class="table__sort-button"
                @click="sortCountries('region', 'ascending')"
                aria-label="Sort ascending"
              >
                ▲
              </button>
              <button
                class="table__sort-button"
                @click="sortCountries('region', 'descending')"
                aria-label="Sort descending"
              >
                ▼
              </button>
            </div>
          </th>
          <th scope="col" :aria-sort="sortColumn == 'Total energy supply' ? sortDirection : null">
            Total energy supply
            <div class="table__sort-button-group">
              <button
                class="table__sort-button"
                @click="sortCountries('Total energy supply', 'ascending')"
                aria-label="Sort ascending"
              >
                ▲
              </button>
              <button
                class="table__sort-button"
                @click="sortCountries('Total energy supply', 'descending')"
                aria-label="Sort descending"
              >
                ▼
              </button>
            </div>
          </th>
          <th scope="col">Member</th>
        </tr>
      </thead>
      <tbody>
        <Country
          v-for="country in currentCountries"
          :key="country.id"
          :index="country.id"
          :countryName="country.country"
          :region="country.region"
          :totalEnergySupply="country['Total energy supply']"
          :member="country['member']"
        />
      </tbody>
    </table>
    <nav class="table-nav" aria-label="Table pagination">
      <button :disabled="currentPage == 1" @click="currentPage--">Previous</button>
      Current page: {{ currentPage }} / {{ totalPages }}
      <button :disabled="currentPage == totalPages" @click="currentPage++">Next</button>
    </nav>
  </main>
</template>
