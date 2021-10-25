<template>
  <div>
    <h1>Выберете город:</h1>
    <select v-model="dep">
      <option
        v-for="city in cities"
        v-bind:key="city.Ref"
        v-bind:value="city.Description"
        >{{ city.Description }}</option
      >
    </select>
    <button v-on:click="checkDeps">Check</button
    ><select>
      <option
        v-for="dep in departments"
        v-bind:key="dep.CityRef"
        v-bind:value="dep.Description"
        >{{ dep.Description }}</option
      >
    </select>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";

export default {
  data: function() {
    return {
      cities: [],
      departments: [],
      dep: "Абрикосівка",
    };
  },
  mounted: function() {
    axios
      .post("https://api.novaposhta.ua/v2.0/json/Address/getCities", {
        apiKey: "0555c556f433266e962e4ded47a8b3e9",
        modelName: "Address",
        calledMethod: "getCities",
        methodProperties: {},
      })
      .then((response) => {
        console.log(response.data);
        this.cities = response.data.data;
      });
  },
  methods: {
    checkDeps: function() {
      axios
        .post("https://api.novaposhta.ua/v2.0/json/Address/getWarehouses", {
          apiKey: "0555c556f433266e962e4ded47a8b3e9",
          modelName: "Address",
          calledMethod: "getWarehouses",
          methodProperties: {
            CityName: this.dep,
          },
        })
        .then((response) => {
          console.log(response.data);
          this.departments = response.data.data;
        });
    },
  },
};
</script>
<style scoped></style>
