<template>
  <div class="row" v-if="city">
    <div class="col-12">
      <div class="row mb-3">
        <div class="col">
          <h1>ВНЖ в городе {{ city.name }}</h1>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <p class="lead">
            Страна контрастов, ярких эмоций и незабываемых впечатлений. Жить
            здесь – сплошное удовольствие, мягкий, но разнообразный климат,
            доброжелательные люди и фееричные восточные сладости.
          </p>
          <p>
            Поможем оформить ВНЖ (Икамет) в Турции в городе {{ city.name }},
            чтобы Вы могли наслаждаться этим дольше 90 дней и без проблем
            привыкли к новой насыщенной жизни в Европе с частичкой восточного
            колорита.
          </p>
        </div>
      </div>
      <div class="row">
        <div class="col d-flex">
          <div class="card">
            <div class="card-body">
              <h5 class="card-title">Комплексный подход</h5>
              <p class="card-text">
                Не нужно обращаться в сторонние центры или искать специалистов
                под каждую ситуацию. Разработаем стратегию получения ВНЖ
                индивидуально исходя из Вашей ситуации.
              </p>
            </div>
          </div>
        </div>
        <div class="col d-flex">
          <div class="card">
            <div class="card-body">
              <h5 class="card-title">Полное сопровождение на всех этапах</h5>
              <p class="card-text">
                Наши специалисты окажут сопровождение и консультацию на каждом
                этапе получения ВНЖ в Турции.
              </p>
            </div>
          </div>
        </div>
        <div class="col d-flex">
          <div class="card">
            <div class="card-body">
              <h5 class="card-title">
                Гарантия возврата денег в случае отказа
              </h5>
              <p class="card-text">
                Если Вам придет отказ на получение ВНЖ, то мы вернем Вам 100%
                стоимости наших услуг.
              </p>
            </div>
          </div>
        </div>
      </div>
      <div class="row mt-5">
        <div class="col">
          <div class="card mb-3 border-primary bg-primary-subtle">
            <div class="card-body">
              <h5 class="card-title">Бесплатная консультация</h5>
              <p class="card-text">
                Оставьте заявку, мы перезвоним Вам в ближайшее время.
              </p>
              <form class="row row-cols-lg-auto g-3 align-items-center">
                <div class="col-12">
                  <label
                    class="visually-hidden"
                    for="inlineFormInputGroupUsername"
                  >
                    Имя
                  </label>
                  <div class="input-group">
                    <input
                      type="text"
                      class="form-control"
                      id="inlineFormInputGroupUsername"
                      placeholder="Имя"
                    />
                  </div>
                </div>
                <div class="col-12">
                  <label
                    class="visually-hidden"
                    for="inlineFormInputGroupUsername"
                  >
                    Телефон
                  </label>
                  <div class="input-group">
                    <input
                      type="text"
                      class="form-control"
                      id="inlineFormInputGroupUsername"
                      placeholder="Телефон"
                    />
                  </div>
                </div>

                <div class="col-12">
                  <button type="submit" class="btn btn-primary">
                    Отправить
                  </button>
                </div>
              </form>
              <p class="mt-2">Или напишите нам в <a href="#">telegram</a></p>
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

    onMounted(async () => {
      contentQuery.value = await queryContent("cities").findOne();
      cities.value = contentQuery.value.body;
      findCity(route);
    });

    return {
      route,
      city,
    };
  },
};
</script>

<style scoped></style>
