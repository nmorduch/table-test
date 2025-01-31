<script setup>
import Country from './components/Country.vue'
import data from './assets/data.json'
</script>

<template>
  <header>
    <h1>Web Developer Test: Sortable Table</h1>
  </header>

  <main>
    <table>
      <thead>
        <tr>
          <th>Country</th>
          <th>Region</th>
          <th>Total energy supply</th>
          <th>Member</th>
        </tr>
      </thead>
      <tbody>
        <Country
          v-for="country in data"
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
