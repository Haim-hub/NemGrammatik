<script setup>
import HelloWorld from './components/HelloWorld.vue'
import MainSection from './components/MainSection.vue';
</script>

<template>
  <header>
    <!-- <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" /> -->

    <!-- <div>
      <HelloWorld msg="You did <br /> it!" />
    </div> -->
  </header>

  <main>
    <MainSection/>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}



@media (min-width: 1024px) {
  header {
    place-content: center;
    display: flex;
    place-items: center;
    padding-bottom: 2em;
  }

  .logo {
    margin: 0 2rem 0 0;
  }

}
</style>
