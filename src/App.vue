<template>
  <app-table></app-table>
</template>

<script>
import appTable from "./components/table";

export default {
  name: "App",
  components: {
    appTable
  },
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
