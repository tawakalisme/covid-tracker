<template>
  <main v-if="!isLoading" class="home">
    <data-title :text="title" :dataDate="dataDate" />
    <data-boxes :stats="stats" />
    <country-select :countries="countries" @get-country="getCountryData" />
  </main>
  <main v-else class="flex flex-col align-center justify-center text-center">
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching data...</div>
    <img :src="loadingImage" alt="loading" class="w-24 h-24 m-auto" />
  </main>
</template>

<script>
import DataTitle from "@/components/DataTitle";
import DataBoxes from "@/components/DataBoxes";
import CountrySelect from "@/components/CountrySelect";
export default {
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  name: "Home",
  data() {
    return {
      isLoading: true,
      title: "Global",
      dataDate: "",
      stats: {},
      countries: [],
      loadingImage: require("../assets/loading.gif"),
    };
  },
  async created() {
    const data = await this.fetchCovidData();

    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.isLoading = false;
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");

      const data = await res.json();
      return data;
    },
    getCountryData(country) {
      if (country === 0) {
        this.toGlobal();
      } else {
        this.stats = country;
        this.title = country.Country;
      }
    },
    async toGlobal() {
      this.loading = true;
      const data = await this.fetchCovidData();
      this.title = "Global";
      this.stats = data.Global;
      this.loading = false;
    },
  },
};
</script>
