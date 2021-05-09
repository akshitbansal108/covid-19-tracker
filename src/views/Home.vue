<template>
  <main v-if="!loading">
    <Title :title="title" :dataDate="dataDate" />
    <Body :stats="stats" />
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

export default {
  name: "Home",
  components: {
    Title,
    Body,
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
