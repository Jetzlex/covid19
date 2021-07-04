<template>
  <div>
    <div
      v-if="selectedCountry == null"
      class="
        animate__animated animate__zoomIn
        min-w-screen
        h-screen
        fixed
        left-0
        top-0
        flex
        justify-center
        items-center
        inset-0
        z-50
      "
    >
      <div
        class="w-full max-w-lg p-5 relative mx-auto my-auto rounded-lg bg-white"
      >
        <p class="font-semibold text-xl">Select a country...</p>
        <br />
        <div class="w-full">
          <div class="mt-3 relative">
            <button
              @click="openDropdown = !openDropdown"
              type="button"
              class="
                border border-gray-100
                w-full
                rounded-lg
                py-3
                px-5
                focus:border-blue-500 focus:ring
                ring-blue-50
                focus:outline-none
              "
            >
              <span class="flex items-center">
                <span
                  :class="{
                    'text-black':
                      dropdownPlaceholder != 'Please select a country...',
                  }"
                  class="block truncate text-gray-400"
                >
                  {{ dropdownPlaceholder }}
                </span>
              </span>
              <span
                class="
                  ml-3
                  absolute
                  inset-y-0
                  right-0
                  flex
                  items-center
                  pr-2
                  pointer-events-none
                "
              >
                <svg
                  class="h-5 w-5 text-gray-400"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                  aria-hidden="true"
                >
                  <path
                    fill-rule="evenodd"
                    d="M10 3a1 1 0 01.707.293l3 3a1 1 0 01-1.414 1.414L10 5.414 7.707 7.707a1 1 0 01-1.414-1.414l3-3A1 1 0 0110 3zm-3.707 9.293a1 1 0 011.414 0L10 14.586l2.293-2.293a1 1 0 011.414 1.414l-3 3a1 1 0 01-1.414 0l-3-3a1 1 0 010-1.414z"
                    clip-rule="evenodd"
                  ></path>
                </svg>
              </span>
            </button>
            <div
              v-if="openDropdown"
              class="absolute mt-1 w-full z-10 rounded-md bg-white shadow-lg"
            >
              <ul
                class="
                  max-h-56
                  rounded-md
                  text-base
                  ring-1
                  p-2
                  ring-black ring-opacity-5
                  overflow-auto
                  focus:outline-none
                  sm:text-sm
                "
              >
                <li
                  class="
                    text-gray-900
                    cursor-default cursor-pointer
                    hover:bg-gray-50
                    rounded-xl
                    select-none
                    relative
                    py-2
                    pl-3
                    pr-9
                  "
                  @click="
                    country = 'GLOBAL'
                    dropdownPlaceholder = 'Global'
                    closeDropdown()
                  "
                >
                  Global
                </li>
                <li
                  v-for="(c, key) in countries"
                  :key="key"
                  class="
                    text-gray-900
                    cursor-default cursor-pointer
                    hover:bg-gray-50
                    rounded-xl
                    select-none
                    relative
                    py-2
                    pl-3
                    pr-9
                  "
                  @click="
                    country = c.ISO2
                    dropdownPlaceholder = c.Country
                    closeDropdown()
                  "
                >
                  {{ c.Country }}
                </li>
              </ul>
            </div>
          </div>
        </div>
        <button
          @click="selectedCountry = country"
          class="
            bg-gray-900
            hover:bg-gray-800
            focus:ring
            ring-offset-2 ring-gray-900
            focus:outline-none
            rounded-lg
            py-3
            px-5
            text-white
            mt-6
            ml-auto
            block
          "
        >
          Select
        </button>
      </div>
    </div>
    <div v-if="selectedCountry">
      <div class="mx-auto mt-16 max-w-5xl">
        <loading :active="!isFetched" :is-full-page="true" color="#acabab" />
        <div v-if="countryData">
          <div>
            <select
              v-model="selectedCountry"
              class="
                border
                cursor-pointer
                focus:outline-none focus:border-indigo-500
                border-gray-200
                bg-white
                rounded-lg
                py-3
                px-5
                w-full
              "
            >
              <option :value="countryData.CountryCode">
                {{ countryData.Country }}
              </option>
              <option v-for="(reqCountry, key) in countries" :key="key">
                {{ reqCountry.Country }}
              </option>
            </select>
            <div class="md:grid md:grid-cols-3">
              <div class="rounded-lg border border-gray-100 p-4 bg-white">
                12
              </div>
            </div>
          </div>
        </div>
        <div v-if="isFetched && !countryData">
          <div
            class="
              bg-red-100
              border border-red-500
              rounded-lg
              py-4
              px-10
              text-red-500 text-center
            "
          >
            An error occurred while processing country data. Please refresh the
            page and try again.
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import vSelect from 'vue-select'
import 'vue-select/dist/vue-select.css'
import Loading from 'vue-loading-overlay'
import 'vue-loading-overlay/dist/vue-loading.css'
export default {
  head: {
    title: 'Covid-19 Stats',
  },
  components: {
    vSelect,
    Loading,
  },
  data() {
    return {
      countries: null,
      country: null,
      openDropdown: false,
      dropdownPlaceholder: 'Please select a country...',
      selectedCountry: null,
      countryData: null,
      isFetched: false,
    }
  },
  watch: {
    country(val) {
      console.log(val.value)
    },
  },
  mounted() {
    let countries = []
    axios.get('https://api.covid19api.com/countries').then((res) => {
      res.data.forEach((data) => {
        countries.push(data)
      })
    })
    this.countries = countries
  },
  methods: {
    closeDropdown() {
      this.openDropdown = false
    },
  },
  watch: {
    selectedCountry(val) {
      this.isFetched = false
      setTimeout(() => {
        axios.get('https://api.covid19api.com/summary').then((res) => {
          if (this.selectedCountry == 'GLOBAL') {
            this.countryData = res.data.Global
          } else {
            res.data.Countries.forEach((data) => {
              if (data.CountryCode == this.selectedCountry) {
                this.countryData = data
              }
            })
          }
          this.isFetched = true
        })
      }, 700)
    },
    countryData(val) {
      console.log(val)
    },
  },
}
</script>

<style lang="postcss">
body {
  @apply bg-gray-100;
}
</style>
