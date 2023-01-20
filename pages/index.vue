<template>
  <div>
    <div>
      <NuxtLink to='/weather'>Check Weather ☂️</NuxtLink>
    </div>
    <div>
      <NuxtLink to="/name">Guess gender by name 👩‍🦳👨🏼‍🦳</NuxtLink>
    </div>
    <div class="connection-status">
      {{
  isOffline?
      '🔴 You are offline. Please check your internet connection.':
    '🟢 You are connected to Internet!Yeah!'
      }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      isOffline: false
    }
  },
  mounted() {
    this.isOffline = !navigator.onLine
  }
}
</script>

<style scoped>
.connection-status {
  margin-top: 40px;
}
</style>
