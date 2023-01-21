<script setup>
import {onMounted} from "vue";


const getFeatures = () => {
  fetch("https://caniuse.rs/features.json", {mode: 'cors'})
      .then((response) => response.json())
      .then((data) => console.log(data))
      .catch((error) => console.log(error));
}

onMounted(() => {
  getFeatures();
});

</script>

<template>
</template>

<script>
export default {
  props: {
    search_input: String,
    features: {
      type: Object,
      default: {}
    }
  },
  name: "Caniusers"
}
</script>

<style scoped>

</style>