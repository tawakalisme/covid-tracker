<template>
  <div
    class="font-bold text-gray-600 text-xs leading-8 uppercase mt-4"
  >
    Select Data to Show
  </div>
  <div class="flex flex-col md:flex-row">
    <div class="w-full flex-1">
      <select
        id="selectCountry"
        @change="onChange()"
        v-model="selected"
        class="form-select block w-full border rounded p-2 mt-2 focus:outline-none"
      >
        <optgroup label="Global Data">
          <option value="0">Global</option>
        </optgroup>
        <optgroup label="Countries">
          <option
            v-for="country in countries"
            :key="country.ID"
            :value="country.ID"
          >
            {{ country.Country }}
          </option>
        </optgroup>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  name: "CountrySelect",
  props: ["countries"],
  data() {
    return {
      selected: 0,
    };
  },
  emits: ["get-country"],
  methods: {
    onChange() {
      const country = this.countries.find((item) => item.ID === this.selected);
      if (country === undefined) {
        this.$emit("get-country", 0);
      } else {
        this.$emit("get-country", country);
      }
    },
  },
};
</script>