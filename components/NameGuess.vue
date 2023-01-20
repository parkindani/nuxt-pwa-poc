<template>
  <div>
    <h2>Let's guess gender based on Name</h2>
    <div v-if="nameDetails" class="universities-list">
      <h3>{{ nameDetails.name }}</h3>
      <p>{{ nameDetails.gender === 'male' ? '👨🏼‍🦳' : '👩‍🦳' }}</p>
    </div>
    <h2>
      <ul>
        <li v-for="name in names" :key="name" @click="guessNameGender(name)">
          {{ name }}
        </li>
      </ul>
    </h2>
    <h2>
      <NuxtLink to="/">Home</NuxtLink>
    </h2>
  </div>
</template>

<script>

export default {
  name: 'NameGuess',
  data() {
    return {
      nameDetails: '',
      names: [
        'Karl',
        'Marks',
        'Lena',
        'Sveta',
        'Hanna',
        'Donald',
        'Trump',
        'Vladimir',
        'Putin',
        'Sarah',
        'Connor',
        'John',
        'Tom',
        'Jerry',
      ]
    }
  },
  methods: {
    guessNameGender(name) {
      this.$axios.get('https://api.genderize.io/?name=' + name).then(response => {
        if (response.status === 200) {
          this.nameDetails = response.data
          // localForage is a wrapper around IndexedDB
          // 데이터를 불러오는데 성공하면, 데이터를 저장한다.
          this.$localForage.setItem(name, response.data)
        } else {
          this.$localForage.getItem(name).then(data => {
            this.nameDetails = data
          })
        }
      }).catch(e => {
        // eslint-disable-next-line no-console
        console.error('error: ', e)
        this.$localForage.getItem(name).then(data => {
          this.nameDetails = data
        })
      })
    }
  },
}
</script>


<style>
.nuxt-logo {
  height: 180px;
}
</style>
