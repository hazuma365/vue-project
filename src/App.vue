<script setup>
import HelloWorld from './components/HelloWorld.vue'
import JsonTableWithSearch from './components/JsonTableWithSearch.vue';
</script>

<template>
  <div class="container">
    <header>
      <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
      <div class="wrapper">
        <HelloWorld msg="JSON Table test" />
      </div>
    </header>

    <main>
      <JsonTableWithSearch/>
    </main>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
}

header {
  line-height: 1.5;
  text-align: center;
  margin-bottom: 2rem;
  width: 100%;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

main {
  width: 100%;
}

@media (min-width: 1024px) {
  header {
    padding-right: 0;
  }

  header .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
}
</style>