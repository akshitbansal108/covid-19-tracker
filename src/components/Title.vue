<template>
  <div class="text-center">
    <div class="text-3xl font-bold">
      {{ title }}
    </div>
    <div class="text-2xl mt-4 mb-10">
      {{ formattedDate }}
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "Title",
  props: ["title", "dataDate"],
  computed: {
    formattedDate: function() {
      return moment(this.dataDate).format("MMMM Do YYYY, h:mm:ss a");
    },
  },
};
</script>
