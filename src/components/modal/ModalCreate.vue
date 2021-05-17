<template>
  <Modal :headerTitle="headerTitle">
    <form @submit.prevent="addStudent">
      <div class="w-full shadow rounded py-6 px-4">
        <div class="grid grid-flow-row">
          <div class="grid grid-flow-row">
            <label class="text-xl mb-2">Nama</label>
            <input
              type="text"
              class="border border-gray-300 py-2 px-3 focus:outline-none rounded focus:ring focus:ring-offset-blue-400 mb-6"
              v-model="nama"
            />
          </div>
          <div class="grid grid-flow-row">
            <label class="text-xl mb-2">Kelas</label>
            <input
              type="text"
              class="border border-gray-300 py-2 px-3 focus:outline-none rounded focus:ring focus:ring-offset-blue-400 mb-6"
              v-model="kelas"
            />
          </div>
          <div class="flex justify-end">
            <button
              class="px-4 py-2 text-gray-50 bg-green-500 hover:bg-green-400 duration-200 rounded"
            >
              Submit
            </button>
          </div>
        </div>
      </div>
    </form>
  </Modal>
</template>

<script>
import Modal from "./Modal.vue";

export default {
  props: ["headerTitle"],
  components: { Modal },
  data() {
    return {
      nama: "",
      kelas: "",
    };
  },
  methods: {
    addStudent() {
      const student = {
        nama: this.nama,
        kelas: this.kelas,
      };
      this.$emit("newStudent", student);
      this.$emit("closeModal");
      fetch("https://sarunk-vue-api.herokuapp.com/api/student", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(student),
      })
        .then((res) => res.json())
        .then((data) => {
          this.$emit("newStudent", data.data);
        });
    },
  },
};
</script>

<style>
</style>