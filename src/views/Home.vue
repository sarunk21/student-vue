<template>
  <div class="container p-5 mx-auto my-5">
    <h1 class="text-3xl font-bold my-3">Student List</h1>
    <router-link
      :to="{ name: 'Create' }"
      class="btn bg-green-400 hover:bg-green-300 duration-200"
    >
      Add
    </router-link>
    <div class="border border-gray-800 rounded p-5 my-3 overflow-auto">
      <table class="w-full" v-if="students.length">
        <thead class="border-b border-gray-800">
          <tr>
            <th class="px-3 py-2">#</th>
            <td class="px-3 py-2">Nama</td>
            <td class="px-3 py-2">Kelas</td>
            <td class="px-3 py-2 text-center">Aksi</td>
          </tr>
        </thead>
        <tbody class="mt-10">
          <tr v-for="(student, index) in students" :key="student.id">
            <td class="table-col text-center">{{ index + 1 }}</td>
            <td class="table-col">{{ student.nama }}</td>
            <td class="table-col">{{ student.kelas }}</td>
            <td class="table-col flex justify-center">
              <button
                class="btn bg-yellow-400 hover:bg-yellow-300 duration-200"
                @click="editModal(student)"
              >
                Edit
              </button>
              <button
                class="btn bg-red-400 hover:bg-red-300 duration-200"
                @click="deleteStudent(student.id)"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
      <h1 v-else class="text-3xl mx-auto py-3 text-center">
        Data Tidak Tersedia
      </h1>
    </div>
    <Modal v-if="showModal" :student="student" @toggleModal="toggleModal" />
    <Create @nStudent="nStudent" v-if="false" />
  </div>
</template>

<script>
import Modal from "../components/Modal.vue";
import Create from "../views/Create.vue";

export default {
  name: "Home",
  data() {
    return {
      students: [],
      showModal: false,
      student: {},
    };
  },
  components: { Modal, Create },
  methods: {
    deleteStudent(id) {
      const student = { id: id };
      const newStudent = this.students.filter((i) => i.id != id);
      this.students = newStudent;
      fetch(`https://sarunk-vue-api.herokuapp.com/api/student/${id}`, {
        method: "DELETE",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(student),
      })
        .then((res) => res.json())
        .then((data) => {
          console.log(data.message);
        });
    },
    fetchStudent() {
      fetch("https://sarunk-vue-api.herokuapp.com/api/student")
        .then((res) => res.json())
        .then((data) => {
          console.log(data.message);
          this.students = data.data;
        });
    },
    editModal(data) {
      this.showModal = true;
      this.student = data;
    },
    toggleModal() {
      this.showModal = false;
    },
  },
  mounted() {
    this.fetchStudent();
  },
};
</script>

<style scoped>
h1,
p {
  color: #21094e;
}
.table-col {
  @apply px-3 py-2;
}
.btn {
  @apply px-3 py-1 rounded text-gray-50 mx-4;
}
</style>
