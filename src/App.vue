<template>
  <div :class="[{ flexStart: step === 1 }, 'wrapper']">
    <transition name="logoSlide">
      <img src="./assets/logo.svg" class="logo" v-if="step === 1" />
    </transition>
    <transition name="backgroundFade">
      <HeroImage v-if="step === 0" />
    </transition>
    <Claim v-if="step === 0" />
    <SearchInput
      v-model="searchValue"
      @input="handleInput"
      :dark="step === 1"
    />
    <div class="results" v-if="results && !loading && step === 1">
      <Item
        v-for="item in results"
        :item="item"
        :key="item.data[0].nasa_id"
        @click.native="handleModalOpen(item)"
      />
    </div>
    <Modal v-if="modalOpen" :item="modalData" @closeModal="modalOpen = false" />
    <Loader v-if="loading && step === 1" />
  </div>
</template>
<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
import HeroImage from './components/HeroImage';
import Claim from './components/Claim';
import SearchInput from './components/SearchInput';
import Item from './components/Item';
import Modal from './components/Modal';
import Loader from './components/Loader';

const API = 'https://images-api.nasa.gov/search';
export default {
  name: 'App',
  components: {
    HeroImage,
    Claim,
    SearchInput,
    Item,
    Modal,
    Loader,
  },
  data() {
    return {
      modalOpen: false,
      modalData: null,
      loading: false,
      step: 0,
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleModalOpen(item) {
      this.modalOpen = true;
      this.modalData = item;
    },
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

.backgroundFade-enter-active,
.backgroundFade-leave-active {
  transition: opacity 0.5s ease;
}

.backgroundFade-enter,
.backgroundFade-leave-to {
  opacity: 0;
}

.logoSlide-enter-active,
.logoSlide-leave-active {
  transition: margin-top 0.5s ease;
}

.logoSlide-enter,
.logoSlide-leave-to {
  margin-top: -50px;
}

.wrapper {
  position: relative;
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

.logo {
  position: absolute;
  top: 40px;
}

.results {
  margin-top: 50px;
  margin-left: auto;
  margin-right: auto;
  width: 80%;
  display: grid;
  grid-template-columns: 1fr;
  grid-gap: 20px;

  @media (min-width: 768px) {
    grid-template-columns: 1fr 1fr;
  }

  @media (min-width: 1024px) {
    grid-template-columns: 1fr 1fr 1fr;
  }
}
</style>
