<template>
  <div :class="[{flexStart : step === 1}, 'wrapper']">
    <transition name="slide">
      <img src="./assets/logo.svg" class="logo" v-if="step === 1" />
    </transition>
    <transition name="fade">
      <HeroImage v-if="step === 0" />
    </transition>
    <Claim v-if="step === 0" />
    <SearchInput v-model="searchValue" @input="handleInput" :dark="step === 1" />
    <div class="results" v-if="results && !loading && step ===1">
      <Item
        v-for="item in results"
        :item="item"
        :key="item.data[0].nasa_id"
        @click.native="handleModalOpen(item)"
      />
    </div>
    <div class="loader" v-if="step ===1 && loading">
      <div />
    </div>
    <Modal v-if="modalOpen" @closeModal="modalOpen = false" :item="modalItem" />
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
import Claim from '@/components/Claim.vue';
import SearchInput from '@/components/SearchInput.vue';
import HeroImage from '@/components/HeroImage.vue';
import Item from '@/components/Item.vue';
import Modal from '@/components/Modal.vue';

const API = 'https://images-api.nasa.gov/search';

export default {
  name: 'Home',
  components: {
    Claim,
    SearchInput,
    HeroImage,
    Item,
    Modal,
  },
  data() {
    return {
      modalItem: null,
      modalOpen: false,
      loading: false,
      step: 0,
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleModalOpen(item) {
      this.modalItem = item;
      this.modalOpen = true;
    },

    handleInput: debounce(function() {
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

<style lang="scss" >
@import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800&display=swap');

* {
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  font-family: 'Montserrat', sans-serif;
  margin: 0;
  padding: 0;
  color: white;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.slide-enter-active,
.slide-leave-active {
  transition: margin-top 0.3s ease;
}
.slide-enter,
.slide-leave-to {
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
  top: 30px;
}
.search {
  display: flex;
  flex-direction: column;
  width: 250px;
}
.results {
  margin-top: 50px;
  width: 80vw;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 20px;

  @media (min-width: 768px) {
    grid-template-columns: repeat(3, 1fr);
  }
}
label {
  font-family: Montserrat, sans-serif;
}

input {
  height: 30px;
  border: 0;
  border-bottom: 1px solid black;
}

.loader {
  margin-top: 100px;
  color: black;
  display: inline-block;
  position: relative;
  width: 64px;
  height: 64px;
  transform: rotate(45deg);
  transform-origin: 32px 32px;
}
.loader div {
  top: 23px;
  left: 19px;
  position: absolute;
  width: 26px;
  height: 26px;
  background: black;
  animation: loader 1.2s infinite cubic-bezier(0.215, 0.61, 0.355, 1);
}
.loader div:after,
.loader div:before {
  content: ' ';
  position: absolute;
  display: block;
  width: 26px;
  height: 26px;
  background: black;
}
.loader div:before {
  left: -17px;
  border-radius: 50% 0 0 50%;
}
.loader div:after {
  top: -17px;
  border-radius: 50% 50% 0 0;
}
@keyframes loader {
  0% {
    transform: scale(0.95);
  }
  5% {
    transform: scale(1.1);
  }
  39% {
    transform: scale(0.85);
  }
  45% {
    transform: scale(1);
  }
  60% {
    transform: scale(0.95);
  }
  100% {
    transform: scale(0.9);
  }
}
</style>
