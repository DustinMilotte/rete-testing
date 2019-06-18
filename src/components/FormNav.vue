<template>
  <button v-on:click="$emit('log-json')" id=json-btn>JSON</button>
</template>

<script>
export default {
  name: "FormNav"
}
</script>

<style>
  #json-btn {
    background:chartreuse;
    position: absolute;
    top:100px;
  }
</style>
