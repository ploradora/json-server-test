<template>
  <nav class="filter-nav">
    <button :class="{ active: current === 'all' }" @click="updateFilter('all')">
      View All
    </button>
    <button
      :class="{ active: current === 'completed' }"
      @click="updateFilter('completed')"
    >
      Completed
    </button>
    <button
      :class="{ active: current === 'ongoing' }"
      @click="updateFilter('ongoing')"
    >
      Ongoing
    </button>
  </nav>
</template>
<script>
export default {
  emits: ["filterChange"],
  props: ["current"],
  methods: {
    updateFilter(by) {
      this.$emit("filterChange", by);
    },
  },
};
</script>

<style>
.filter-nav button {
  background: none;
  border: none;
  outline: none;
  color: #bbb;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
  cursor: pointer;
}
.filter-nav .active {
    color: rgb(78, 78, 78);
}
</style>
