<template>
  <main v-if="!loading">
    <Title :title="title" :dataDate="dataDate" />
    <Body :stats="stats" />
    <SelectCountry @get-country-data="onCountryChange" :countries="countries" />
    <button
      v-if="stats.Country"
      @click="clearSelection"
      class="bg-blue-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-blue-600"
    >
      Clear
    </button>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching your request
    </div>
    <!-- : before src is used for binding loadingImage -->
    <img :src="loadingImage" alt="loader" class="w-24 m-auto" />
  </main>
</template>

<script>
import Title from "@/components/Title";
import Body from "@/components/Body";
import SelectCountry from "@/components/SelectCountry";

export default {
  name: "Home",
  components: {
    Title,
    Body,
    SelectCountry,
  },
  data() {
    return {
      loading: true,
      dataDate: "",
      title: "Global",
      countries: [],
      stats: {},
      loadingImage: require("../assets/hourglass.gif"),
    };
  },
  methods: {
    async getData() {
      const covidApiResponse = await fetch(
        "https://api.covid19api.com/summary"
      );
      const covidData = await covidApiResponse.json();
      return covidData;
    },
    onCountryChange(country) {
      this.title = country.Country;
      this.stats = country;
    },
    async clearSelection() {
      this.loading = true;
      const freshData = await this.getData();
      this.title = "Global";
      this.stats = freshData.Global;
      this.loading = false;
    },
  },
  async created() {
    const covidData = await this.getData();

    this.dataDate = covidData.Date;
    this.countries = covidData.Countries;
    this.stats = covidData.Global;
    this.loading = false;
  },
};
</script>
