<template>
  <div class="control">
    <label for="cpicker">Select Country</label>
    <input
      ref="countryPicked"
      type="text"
      id="cpicker"
      @focus="this.$refs.slider.setAttribute('animated', '')"
    />
    <i class="bx bx-search"></i>
    <div class="country-select-wrap">
      <div v-if="countriesLoaded" class="slide-wrap" ref="slider">
        <div
          v-for="country in countries"
          :key="country.code"
          @click="selectCountry"
        >
          <img :src="country.image" :data-country="country.name" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      countries: [],
      countriesLoaded: false,
    };
  },
  mounted() {
    this.getCountries();
  },
  methods: {
    async getCountries() {
      await fetch(
        "https://cdn.jsdelivr.net/npm/country-flag-emoji-json@2.0.0/dist/index.json"
      ).then((data) =>
        data.json().then((res) => {
          this.countries = res;
          this.countriesLoaded = true;
        })
      );
    },
    selectCountry(e) {
      this.$refs.countryPicked.value = e.target.getAttribute("data-country");
      this.$refs.slider.removeAttribute("animated");
      this.$refs.slider.setAttribute("hide","");
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Tajawal:wght@300&display=swap");
body {
  height: 100vh;
  width: 100vw;
  margin: 0;
  display: grid;
  place-items: center;
  background: rgb(82, 85, 82);
  color: White;
  font-family: "Tajawal", sans-serif;
  font-size: 1.2rem;
  overflow: hidden;
}
.control {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  position: relative;

  label {
    padding-left: 0.25rem;
    font-size: 1.4rem;
  }

  input {
    padding: 0.5rem;
    width: 300px;
    border: 0;
    background: white;
    outline: none;
    font-size: 100%;
    border-radius: 0.75rem;
    padding-left: 3rem;
  }

  i {
    position: absolute;
    top: 3rem;
    color: #bdbdbd;
    left: 1rem;
  }

  .country-select-wrap {
    background: rgb(82, 85, 82);
    width: 350px;
    position: relative;
    overflow: hidden;

    .slide-wrap {
      display: flex;
      flex-direction: row;
      transform: translateX(-100%);
      width: 20900px;
      transition: transform 40000ms ease-in-out, opacity 100ms ease-in-out;
      opacity:1;

      &[hide]{
        opacity:0;
      }

      &[animated] {
        transform: translateX(0);
      }

      img {
        height: 5rem;
        cursor: pointer;
        transform:scale(1);
        transition:all 50ms ease-in-out;

        &:hover {
          transform: scale(1.1);
        }
      }
    }
  }
}
</style>
