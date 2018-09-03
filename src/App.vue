<template>
  <div id="app">
    <img :style="{ filter: hueColor(currentHue) }"
         @click="enabled = !enabled"
         :class="{ 'tada animated': enabled }"
         class="logo pulse animated hoverable"
         src="./assets/wal_logo.svg"
         alt="WAŁ"
    >
    <transition name="slide-fade">
      <subscribe-modal v-on:hideModal="showModal=false" v-if="showModal"></subscribe-modal>
    </transition>
    <div class="icon-aggregator bounceInDown animated" :style="{ filter: hueColor(currentHue + 180) }">
      <ul>
        <a target="_blank" href="https://www.facebook.com/wal.407/">
          <li><span class="fab fa-facebook-square fa-lg hoverable"></span></li>
        </a>

        <a target="_blank" href="https://www.instagram.com/wal.407/ ">
          <li><span class="fab fa-instagram fa-lg hoverable"></span></li>
        </a>
      </ul>
    </div>
    <router-view/>
  </div>
</template>

<script>
  import SubscribeModal from './components/SubscribeModal'

  export default {
    name: 'App',
    components: {
      SubscribeModal
    },
    data: function() {
      return {
        currentHue: 1,
        enabled: false,
        ascending: true,
        showModal: false,
      }
    },
    methods: {
      hueColor: (hue) => {
        return 'invert(0.5) sepia(1) saturate(3.5) hue-rotate(' + (360-hue).toString() +'deg)'
      }
    },
    created: function() {
      setInterval(() => {
        if (this.currentHue > 359 || this.currentHue < 1) {
          this.ascending = !this.ascending
        }
        if (this.ascending) {
          this.currentHue += 1;
        } else {
          this.currentHue -= 1;
        }
      }, 100);

      setTimeout(() => {
        this.showModal = true;
      }, 2200)
    }
  }
</script>

<style lang="scss">
  @import "./assets/animate.css";
  @import url('https://fonts.googleapis.com/css?family=Montserrat');

  * {
    transition: all 0.6s;
  }

  /*slide*/
  .slide-fade-enter-active {
    transition: all 1s ease;
  }
  .slide-fade-leave-active {
    transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-fade-enter, .slide-fade-leave-to
    /* .slide-fade-leave-active below version 2.1.8 */ {
    transform: translatey(-100%);
    opacity: 0;
  }

  .logo {
    grid-column: 2;
    grid-row: 2;
  }

  .fab {
    font-size: 5rem;
  }

  .hoverable {
    &:hover {
      cursor: pointer;
      opacity: 0.82;
    }
  }

  .icon-aggregator {
    grid-column: 2 / 3;
    grid-row: 3;

    justify-content: center;
    align-content: center;
    justify-items: center;
    align-items: center;
    ul {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      grid-template-rows: 5rem;
      justify-items: center;
      align-items: center;

      list-style-type: none;
      li {
        display: inline-block;
      }
    }
  }

  html {
    background-image: linear-gradient(to right, rgba(255, 167, 84, 0.63), rgba(101, 255, 18, 0.68));
    font-family: Montserrat, sans-serif;
  }
  #app {
    display: grid;
    grid-template-columns: 1fr 500px 1fr;
    grid-template-rows: 1fr 500px 6rem 1fr;
  }

  @media only screen and (max-width: 750px) {
    #app {
      grid-template-rows: 1fr 300px 6rem 1fr;
      grid-template-columns: 1fr 260px 1fr;
    }

    .logo {
      width: 19rem;
      height: auto;
    }
  }
</style>
