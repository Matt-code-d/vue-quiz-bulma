<template>
  <div id="app" class="centered-container">
    <GreetingCard />
  </div>
  <FooterApp />
</template>

<script>
import GreetingCard from "@/components/GreetingCard.vue";
import FooterApp from "@/components/FooterApp.vue";

export default {
  name: "App",
  components: {
    GreetingCard,
    FooterApp,
  },
};
</script>

<style>
@import "~bulma/css/bulma.css";

/* Stile personalizzato per il contenitore centrato */
.centered-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh; /* Altezza minima per coprire l'intera viewport verticalmente */
  text-align: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.gamekey:hover {
  cursor: pointer;
}
</style>