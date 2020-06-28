<template>
  <div class="searchWrapper">
    <input
      id="search"
      name="search"
      v-model="searchValue"
      @input="handleInput"
    />
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

  input {
    height: 30px;
    color: white;
    text-align: center;
    font-size: 18px;
    font-weight: 300;
    border: 0;
    border-bottom: 1px solid white;
    background: none;
  }

  input:focus {
    outline: none;
    box-shadow: 0 5px 10px -8px white;
  }
}
</style>
