<template>
  <div class="app">
    <div :class="[{ flexStart: step === 1 }, 'wrapper']">
      <transition>
        <HeroImage v-if="step === 0" />
      </transition>
      <Claim v-if="step === 0" />
      <SearchInput
        v-model="searchValue"
        @input="handleInput"
        :dark="step === 1"
      />
    </div>
  </div>
</template>
<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
import HeroImage from './components/HeroImage';
import Claim from './components/Claim';
import SearchInput from './components/SearchInput';

const API = 'https://images-api.nasa.gov/search';
export default {
  name: 'App',
  components: {
    HeroImage,
    Claim,
    SearchInput,
  },
  data() {
    return {
      loading: false,
      step: 0,
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleInput: debounce(function fetchApi() {
      this.loading = true;
      axios
        .get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
          this.loading = false;
          this.step = 1;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
  },
};
</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800');

* {
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  font-family: 'Montserrat', sans-serif;
  margin: 0;
  padding: 0;
}

.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0;
  padding: 30px;
  width: 100%;
  height: 100vh;

  &.flexStart {
    justify-content: flex-start;
  }
}
</style>
