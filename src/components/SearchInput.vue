<template>
  <div class="searchWrapper">
    <input
      id="search"
      name="search"
      :class="{ dark }"
      :value="value"
      @input="handleChange"
    />
  </div>
</template>
<script>
export default {
  name: 'SearchInput',
  props: {
    value: {
      type: String,
      required: true,
    },
    dark: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    handleChange(e) {
      this.$emit('input', e.target.value);
    },
  },
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
    transition: box-shadow 0.5s ease-out;
    background: none;

    @media (min-width: 1024px) {
      font-weight: 400;
    }
  }

  input:focus {
    outline: none;
    box-shadow: 0 8px 10px -8px white;
  }

  .dark {
    color: #1e3d4a;
    border-bottom-color: #1e3d4a;
  }

  dark:focus {
    box-shadow: 0 8px 10px -8px #1e3d4a;
  }
}
</style>
