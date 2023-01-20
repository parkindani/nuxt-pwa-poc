<template>
  <div>
    <h2>Universities</h2>
    <div v-if="universities" class="universities-list">
      <p>{{ JSON.stringify(universities) }}</p>
    </div>
    <h2>
      <ul>
        <li v-for="country in countries" :key="country" @click="fetchUniversities(country)">
          {{ country }}
        </li>
      </ul>
    </h2>

  </div>
</template>

<script>

export default {
  name: 'UniversitiesList',
  data() {
    return {
      universities: [],
      selectedUniversity: null,
      countries: [
        'France',
        'Japan',
        'Germany'
      ]
    }
  },
  methods: {
    fetchUniversities(country) {
      // eslint-disable-next-line no-console
      console.log(country)
      // const response = await this.$axios.get('http://universities.hipolabs.com/search', { country })
      // this.universities = response.data
    }
  },
}
</script>


<style>
.nuxt-logo {
  height: 180px;
}
</style>
