<template>
  <div>
    <div v-if="typeof bulb != 'undefined'" class="bulb" :class="{ on: bulb }"> {{ value }} </div>
    <div v-if="diagonal.main || diagonal.second" class="diagonal" :class="diagonal.main ? 'main' : 'second'" />
  </div>
</template>

<script>
export default {
  name: "cell",
  props: {
    value: { type: Number },
    bulb: { type: [Boolean, Number] },
    main: { type: Boolean, default: false },
    diagonal: { type: Object, default: { main: false, second: false } }
  },
  data: () => ({}),
  computed: {},
  mounted() {},
  methods: {}
};
</script>
