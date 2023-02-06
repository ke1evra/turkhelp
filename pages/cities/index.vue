<template>
  <div class="row">
    <div class="col">
      <h1>Города турции</h1>
    </div>
  </div>
  <div class="row mt-5">
    <div class="col">
      <ul class="nav nav-tabs mb-3" id="pills-tab" role="tablist">
        <li class="nav-item" role="presentation">
          <button
            class="nav-link active"
            id="pills-home-tab"
            data-bs-toggle="pill"
            data-bs-target="#pills-home"
            type="button"
            role="tab"
            aria-controls="pills-home"
            aria-selected="true"
          >
            По алфавиту
          </button>
        </li>
        <li class="nav-item" role="presentation">
          <button
            class="nav-link"
            id="pills-profile-tab"
            data-bs-toggle="pill"
            data-bs-target="#pills-profile"
            type="button"
            role="tab"
            aria-controls="pills-profile"
            aria-selected="false"
          >
            По району
          </button>
        </li>
      </ul>
      <div class="tab-content" id="pills-tabContent">
        <div
          class="tab-pane fade show active"
          id="pills-home"
          role="tabpanel"
          aria-labelledby="pills-home-tab"
          tabindex="0"
        >
          <div class="row mt-5" v-for="item in citiesByChar">
            <div class="col-12">
              <h4>{{ item.char }}</h4>
              <div class="row">
                <div class="col-2" v-for="city of item.cities" :key="city.name">
                  <a :href="renderLink(city)">{{ city.name }}</a>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div
          class="tab-pane fade"
          id="pills-profile"
          role="tabpanel"
          aria-labelledby="pills-profile-tab"
          tabindex="0"
        >
          <div class="row mt-5" v-for="item in citiesByIl">
            <div class="col-12">
              <h4>{{ item.il }}</h4>
              <div class="row">
                <div class="col-2" v-for="city of item.cities" :key="city.name">
                  <a :href="renderLink(city)">{{ city.name }}</a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { onMounted } from "#imports";
import { Ref } from "@vue/reactivity";
import { ICity } from "~/types";

interface IAlphabeticCities {
  char: string;
  cities: ICity[];
}

interface ICitiesByIl {
  il: string;
  cities: ICity[];
}
export default {
  setup() {
    const contentQuery = ref();
    const cities: Ref<ICity[]> = ref([]);
    const citiesByChar: Ref<IAlphabeticCities[]> = ref([]);
    const citiesByIl: Ref<ICitiesByIl[]> = ref([]);

    const renderLink = (city: ICity) => {
      return `cities/${city.nameEng}`;
    };

    const renderCitiesByChar = (cities: ICity[]): IAlphabeticCities[] => {
      const obj: Record<string, ICity[]> = {};

      cities.forEach((city) => {
        const char = Array.from(city.name)[0];

        if (!obj[char]) {
          obj[char] = [];
        }

        obj[char].push(city);
      });

      return Object.keys(obj)
        .map((char) => {
          return {
            char,
            cities: obj[char],
          };
        })
        .sort((n1, n2) => {
          if (n1.char > n2.char) {
            return 1;
          }
          if (n1.char < n2.char) {
            return -1;
          }
          return 0;
        });
    };

    const renderCitiesByIl = (cities: ICity[]): ICitiesByIl[] => {
      const obj: Record<string, ICity[]> = {};

      cities.forEach((city) => {
        const { il } = city;

        if (!obj[il]) {
          obj[il] = [];
        }

        obj[il].push(city);
      });

      return Object.keys(obj)
        .map((il) => {
          return {
            il,
            cities: obj[il],
          };
        })
        .sort((n1, n2) => {
          if (n1.il > n2.il) {
            return 1;
          }
          if (n1.il < n2.il) {
            return -1;
          }
          return 0;
        });
    };

    onMounted(async () => {
      contentQuery.value = await queryContent("cities").findOne();
      cities.value = contentQuery.value.body;
      citiesByChar.value = renderCitiesByChar(cities.value);
      citiesByIl.value = renderCitiesByIl(cities.value);
    });

    return {
      contentQuery,
      cities,
      renderLink,
      citiesByChar,
      citiesByIl,
    };
  },
};
</script>
