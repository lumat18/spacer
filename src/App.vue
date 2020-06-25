<template>
  <div class="app">
    <div class="nav">
      <router-link to="/">spacer</router-link>
    </div>
    <router-view />
  </div>
</template>

<style lang="scss" scoped>
* {
  box-sizing: border-box;
}
body {
  font-family: 'Montserrat', sans-serif;
}
</style>
