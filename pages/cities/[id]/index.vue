<template>
  <div class="city-page" v-if="city">
    <div class="row">
      <div class="col">
        <h1>{{ city.name }}</h1>
        <table class="table">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">name</th>
              <th scope="col">nameEng</th>
              <th scope="col">il</th>
              <th scope="col">ilEng</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <th scope="row">{{ city.index }}</th>
              <td>{{ city.name }}</td>
              <td>{{ city.nameEng }}</td>
              <td>{{ city.il }}</td>
              <td>{{ city.ilEng }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <a :href="renderLink($route, '/residence')">ВНЖ</a>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { onMounted } from "#imports";
import { Ref } from "@vue/reactivity";
import { ICity } from "~/types";
import { RouteLocationNormalizedLoaded } from "vue-router";

export default {
  setup() {
    const route = useRoute();
    const contentQuery = ref();
    const cities: Ref<ICity[]> = ref([]);
    const city: Ref<ICity | undefined> = ref();

    const findCity = (route: RouteLocationNormalizedLoaded) => {
      const {
        params: { id },
      } = route;

      city.value = cities.value.find((item) => item.nameEng === id);
    };

    const renderLink = (route: RouteLocationNormalizedLoaded, slug: string) => {
      return route.fullPath + slug;
    };

    onMounted(async () => {
      contentQuery.value = await queryContent("cities").findOne();
      cities.value = contentQuery.value.body;
      findCity(route);
    });

    return {
      route,
      city,
      renderLink,
    };
  },
};
</script>

<style scoped></style>
