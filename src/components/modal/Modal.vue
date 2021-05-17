<template>
  <div
    class="w-full h-full fixed left-0 top-0 bg-gray-900 bg-opacity-90 p-3"
    @click.self="closeModal"
  >
    <div
      class="container mt-10 mx-auto bg-gray-50 w-full lg:w-2/3 grid grid-flow-row rounded"
    >
      <div class="border-b border-gray-700 flex justify-between items-center">
        <h1 class="font-semibold text-2xl p-5 text-gray-700">
          {{ headerTitle }}
        </h1>
        <button
          class="font-semibold text-2xl py-5 px-8 text-gray-700"
          @click="closeModal"
        >
          X
        </button>
      </div>
      <div class="p-5 text-gray-700">
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["headerTitle"],
  methods: {
    closeModal() {
      this.$emit("closeModal");
    },
  },
};
</script>