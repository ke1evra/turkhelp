<template>
  <pre v-show="false">
    {{ cities }}
  </pre>
  <ul class="list-group">
    <li class="list-group-item" v-for="city of cities" :key="city.name">
      <a :href="renderLink(city)">{{ city.name }}</a>
    </li>
  </ul>
</template>

<script lang="ts">
import { onMounted } from "#imports";
import { Ref } from "@vue/reactivity";
import { ICity } from "~/types";
export default {
  setup() {
    const contentQuery = ref();
    const cities: Ref<ICity[]> = ref([]);

    const renderLink = (city: ICity) => {
      return `cities/${city.nameEng}`;
    };

    onMounted(async () => {
      contentQuery.value = await queryContent("cities").findOne();
      cities.value = contentQuery.value.body;
    });

    return {
      contentQuery,
      cities,
      renderLink,
    };
  },
};
</script>
