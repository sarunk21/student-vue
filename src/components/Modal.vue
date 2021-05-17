<template>
  <div
    class="w-full h-full top-0 left-0 fixed bg-gray-800 bg-opacity-80 p-10"
    @click.self="toggleModal"
  >
    <div class="container mx-auto bg-gray-50 w-full lg:w-2/4 p-5 rounded">
      <div class="container mx-auto p-5 grid grid-flow-row rounded">
        <form @submit.prevent="editStudentAPI">
          <div class="grid grid-flow-row my-3">
            <label for="nama">Nama</label>
            <input
              type="text"
              id="nama"
              class="p-2 border-2 border-green-400 rounded"
              v-model="student.nama"
            />
          </div>
          <div class="grid grid-flow-row my-3">
            <label for="kelas">Kelas</label>
            <input
              type="text"
              id="kelas"
              class="p-2 border-2 border-green-400 rounded"
              v-model="student.kelas"
            />
          </div>
          <div class="grid grid-flow-row my-3">
            <button
              class="bg-green-400 hover:bg-green-300 py-2 rounded-lg text-white"
            >
              Submit
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["student"],
  methods: {
    editStudentAPI() {
      const updateStudent = {
        nama: this.student.nama,
        kelas: this.student.kelas,
      };
      this.$emit("toggleModal", updateStudent);
      fetch(
        `https://sarunk-vue-api.herokuapp.com/api/student/${this.student.id}`,
        {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(updateStudent),
        }
      );
    },
    toggleModal() {
      this.$emit("toggleModal");
    },
  },
};
</script>

<style>
</style>