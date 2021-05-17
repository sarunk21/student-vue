<template>
  <Modal :headerTitle="headerTitle">
    <form @submit.prevent="updateStudent" v-if="student.id">
      <div class="w-full shadow rounded py-6 px-4">
        <div class="grid grid-flow-row">
          <div class="grid grid-flow-row">
            <label class="text-xl mb-2">Nama</label>
            <input
              type="text"
              class="border border-gray-300 py-2 px-3 focus:outline-none rounded focus:ring focus:ring-offset-blue-400 mb-6"
              v-model="student.nama"
            />
          </div>
          <div class="grid grid-flow-row">
            <label class="text-xl mb-2">Kelas</label>
            <input
              type="text"
              class="border border-gray-300 py-2 px-3 focus:outline-none rounded focus:ring focus:ring-offset-blue-400 mb-6"
              v-model="student.kelas"
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
    <ShimmerModal v-if="student.id == null" />
  </Modal>
</template>

<script>
import Modal from "./Modal.vue";
import ShimmerModal from "../shimmer/ShimmerModal.vue";

export default {
  props: ["headerTitle", "studentId"],
  data() {
    return {
      student: {},
    };
  },
  components: { Modal, ShimmerModal },
  methods: {
    updateStudent() {
      const student = {
        id: this.studentId,
        nama: this.student.nama,
        kelas: this.student.kelas,
      };
      this.$emit("editStudent", student);
      this.$emit("closeModal");
      fetch(
        `https://sarunk-vue-api.herokuapp.com/api/student/${this.studentId}`,
        {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(student),
        }
      )
        .then((res) => res.json())
        .then((data) => {});
    },
  },
  mounted() {
    fetch(`https://sarunk-vue-api.herokuapp.com/api/student/${this.studentId}`)
      .then((res) => res.json())
      .then((data) => {
        this.student = data.data;
      });
  },
};
</script>

<style>
</style>