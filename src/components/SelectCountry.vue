<template>
  <select
    @change="onSelectingCountry()"
    v-model="selectedCountry"
    class="form-select mt-10 border block w-full p-3 rounded"
  >
    <option value="0">Select Country </option>
    <option v-for="country in countries" :value="country.ID" :key="country.ID"
      >{{ country.Country }}
    </option>
  </select>
</template>

<script>
export default {
  name: "SelectCountry",
  props: ["countries"],
  data() {
    return {
      selectedCountry: 0,
    };
  },
  methods: {
    // emit is used when we want to emit changes upwards
    onSelectingCountry() {
      const countryDetails = this.countries.find(
        (country) => country.ID === this.selectedCountry
      );
      this.$emit("get-country-data", countryDetails);
    },
  },
};
</script>
