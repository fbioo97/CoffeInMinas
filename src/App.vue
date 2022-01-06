<template>
  <div>
    <Navbar :logo="caneca_src" :alt="app_name" />
    <div>
      <router-view></router-view>
    </div>
    <Footer />
  </div>
</template>
<script>
import Navbar from './components/Navbar'
import Footer from './components/Footer'
export default {
  data: function() {
    return {
      caneca_src: "/img/caneca.png",
      app_name: "Coffe in Minas"
    }
  },
  components: {
    Navbar,
    Footer
  }
}
</script>

<style>
  * {
    font-family: Helvetica;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }
  .main-container {
    margin: 50px;
    min-height: 250px;
  }
  h1 {
    text-align: center;
    font-size: 42px;
    margin-bottom: 10px;
    color: #222;
  }
  
  h2 {
    text-align: center;
    font-size: 15px;
    margin-bottom: 50px;
    color: #222;
  }
</style>