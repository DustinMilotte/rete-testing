<template>
  <button v-on:click="$emit('log-json')" id="json-btn">Submit JSON</button>
</template>

<script>
export default {
  name: "JsonSubmit"
};
</script>

<style>
  #json-btn {
    position: absolute;
    top:100px;
    right: 100px;
    background: #fff;
    border: 1px solid #ddd;
    padding: .5rem 1.25rem;
    border-radius: 5px;
    cursor: pointer;
    color: #444;
  }
  #json-btn:hover {
        box-shadow: 0 2px 5px 0 
  }
</style>
