<template>
  <nav
    class="navbar navbar-expand-lg bg-body-tertiary bg-dark"
    data-bs-theme="dark"
  >
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
      </div>
    </div>
  </nav>
</template>

<script lang="ts">
import { ReactiveVariable } from "vue/macros";
import { IHeaderLink } from "~/types";

export default {
  setup() {
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

    return {
      links,
    };
  },
};
</script>
