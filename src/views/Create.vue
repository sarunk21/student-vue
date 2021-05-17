<template>
  <div class="container p-5 mx-auto my-5">
    <div
      class="container mx-auto w-full lg:w-2/4 p-5 grid grid-flow-row border border-gray-800 rounded my-4"
    >
      <h1 class="text-3xl">Form Add Student</h1>
      <form @submit.prevent="addStudent">
        <div class="grid grid-flow-row my-3">
          <label for="nama">Nama</label>
          <input
            type="text"
            id="nama"
            class="p-2 border-2 border-green-400 rounded"
            v-model="nama"
          />
        </div>
        <div class="grid grid-flow-row my-3">
          <label for="kelas">Kelas</label>
          <input
            type="text"
            id="kelas"
            class="p-2 border-2 border-green-400 rounded"
            v-model="kelas"
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
</template>

<script>
export default {
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
      fetch("https://sarunk-vue-api.herokuapp.com/api/student", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(student),
      })
        .then((res) => res.json())
        .then((data) => {
          this.$router.push("/");
        });
    },
  },
};
</script>

<style>
</style>