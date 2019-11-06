<template>
  <div class="searchWrapper">
    <input name="search" id="search" v-model="searchValue" @input="handleInput" />
  </div>
</template>
<script>
export default {
  name: 'SearchInput',
};
</script>

<style lang="scss" scoped>
.searchWrapper {
margin-top: 50px;
  display: flex;
  flex-direction: column;
  width: 250px;
}

input {
    background-color: transparent;
  height: 30px;
  border: 0;
  border-bottom: 1px solid black;
}
</style>
