<template>
  <div>
    <div v-if="typeof bulb != 'undefined'" class="bulb" :class="{ on: bulb }">
      {{ value }}
    </div>
    <div v-if="main | second" class="diagonal" :class="main ? 'main' : 'second'"> </div>
  </div>
</template>

<script>
export default {
  name: "cell",
  props: {
    value: { type: Number },
    bulb: { type: [Boolean, Number] },
    main: { type: Boolean, default: false },
    second: { type: Boolean, default: false }
  },
  data: () => ({}),
  computed: {},
  mounted() {},
  methods: {}
};
</script>
