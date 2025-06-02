<template>
    <div class="layout">
      <header v-if="$slots.header">
        <slot name="header"></slot></header>
      <aside>
        <slot name="aside"></slot></aside>
      <main>
        <slot name="main"></slot></main>
      <footer>
        <slot name="footer"></slot></footer>
    </div>
</template>

<style>
.layout {
  display: grid;
  grid-template-columns: 400px 1fr;
}
.layout > * {
  border: solid 1px darkorange;
}
.layout header,
.layout footer {
  grid-column: 1 / -1;
}
</style>
<script setup lang="ts">
</script>

