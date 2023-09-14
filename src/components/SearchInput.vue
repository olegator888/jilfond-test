<script setup>
defineProps(["search", "onChange"]);
</script>

<template>
  <input :value="search" @input="onChange" class="input" placeholder="Поиск" type="text" />
</template>

<style lang="scss">
  .input {
    width: 100%;
    padding: 16px 24px;
    border-radius: 8px;
    border: 1.5px solid var(--all-colors-gray-gray-200, #E9ECEF);
    background: var(--all-colors-black-white-white, #FFF);
    outline: none;  
    font-family: Montserrat, sans-serif;
    font-size: 14px;
    transition: border 200ms ease;

    &:focus,
    &:hover {
      outline: none;

      border-color: #cccccc;
    }
  }
</style>