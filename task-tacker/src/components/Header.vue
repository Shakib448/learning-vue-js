<template>
  <header>
    <h1>{{ title }}</h1>
  </header>
</template>

<script>
export default {
  name: "Header",
  props: {
    title: String,
  },
};
</script>

<style lang="scss">
header {
  display: flex;
  justify-content: content;
  align-items: center;
  margin-bottom: 20px;
}
</style>
