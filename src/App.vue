<template>
  <router-view :key="$route.fullPath" />
</template>

<script>
export default {
  name: "App",

  data: () => ({}),
  computed: {},
  mounted() {},
  methods: {}
};
</script>

<style lang="scss">
* {
  margin: 0;
  box-sizing: border-box;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
}
</style>
