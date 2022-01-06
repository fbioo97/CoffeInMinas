<template>
  <div>
    <Banner />
    <div class="main-container">
      <h1>Bem Vindo! </h1>
      <h2>Hora do Café :D</h2>
      <CoffeForm />
    </div>
  </div>
</template>

<script>
import Banner from '../components/Banner'
import CoffeForm from '../components/CoffeForm'
export default {
  name: 'Home',
  components: {
    Banner,
    CoffeForm
  }
}
</script>