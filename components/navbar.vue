<template>
  <nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container">
      <a class="navbar-brand" href="/">TurkHelp</a>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item" v-for="link in links" :key="link.name">
            <NuxtLink :to="link.path" class="nav-link" active-class="active">
              {{ link.name }}
            </NuxtLink>
          </li>
        </ul>
        <div class="form-check form-switch">
          <input
            class="form-check-input"
            type="checkbox"
            role="switch"
            id="flexSwitchCheckDefault"
            @click="changeDarkModeState"
            v-model="isDarkMode"
          />
          <label class="form-check-label" for="flexSwitchCheckDefault"
            >Dark mode</label
          >
        </div>
      </div>
    </div>
  </nav>
</template>

<script lang="ts">
import { ReactiveVariable } from "vue/macros";
import { IHeaderLink } from "~/types";
import { onMounted } from "#imports";

export default {
  setup() {
    const isDarkMode = ref(false);

    const updateHtmlAttrs = () => {
      useHead({
        htmlAttrs: {
          "data-bs-theme": isDarkMode.value ? "dark" : "light",
        },
      });
    };

    const changeDarkModeState = () => {
      isDarkMode.value = !isDarkMode.value;

      updateHtmlAttrs();
    };

    const links: ReactiveVariable<IHeaderLink[]> = reactive([
      {
        name: "Города",
        path: "/cities",
      },
      {
        name: "ВНЖ",
        path: "/residence",
      },
    ]);

    onMounted(() => {
      const hours = new Date().getHours();
      if (hours > 18 || hours < 9) {
        isDarkMode.value = true;
        updateHtmlAttrs();
      }
    });

    return {
      links,
      changeDarkModeState,
      isDarkMode,
    };
  },
};
</script>
