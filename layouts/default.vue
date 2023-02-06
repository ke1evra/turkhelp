<template>
  <div class="default-layout">
    <KeepAlive>
      <navbar />
    </KeepAlive>
    <div class="container py-5 flex-grow-1 main-content">
      <slot />
    </div>
    <footer-component />
  </div>
</template>
<style>
.main-content {
  min-height: calc(100vh - 356px);
}
</style>
