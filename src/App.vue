<script setup>
import EstimateForm from './components/EstimateForm.vue'
</script>

<template>
  <EstimateForm />
</template>
<style>
@import "./assets/base.css";

#app {
    max-width: 1280px;
    margin: 0 auto;
    padding: 2rem;
    font-weight: normal;
}

header {
    line-height: 1.5;
}

a,
.green {
    text-decoration: none;
    color: hsla(160, 100%, 37%, 1);
    transition: 0.4s;
}

@media (hover: hover) {
    a:hover {
        background-color: hsla(160, 100%, 37%, 0.2);
    }
}

</style>
