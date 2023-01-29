<template>
  <pre v-show="false">
    {{ cities }}
  </pre>
  <ul class="list-group">
    <li class="list-group-item" v-for="city of cities" :key="city.name">
      <a :href="city.nameEng">{{ city.name }}</a>
    </li>
  </ul>
</template>

<script lang="ts">
import { onMounted } from "#imports";
import { Ref } from "@vue/reactivity";

export default {
  setup() {
    const contentQuery = ref();
    const cities = ref([]);

    onMounted(async () => {
      contentQuery.value = await queryContent().findOne();
      console.log(contentQuery.value.body);
      cities.value = contentQuery.value.body;
    });

    return {
      contentQuery,
      cities,
    };
  },
};
</script>
