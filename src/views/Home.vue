<template>
  <div class="container p-5 mx-auto my-5">
    <h1 class="text-3xl font-bold my-3">Daftar Murid</h1>
    <button
      class="px-4 py-2 rounded text-gray-50 bg-green-400 hover:bg-green-300 duration-200"
      @click="toggleModalCreate"
      v-if="pagination.current_page == 1"
    >
      Add
    </button>
    <div
      class="container mx-auto border border-gray-300 rounded my-3 overflow-auto"
      v-if="students.length"
    >
      <table class="w-full">
        <thead class="border-b border-gray-300">
          <tr>
            <th class="py-3 px-8 text-gray-700 text-left">#</th>
            <th class="py-3 px-8 text-gray-700 text-left">Nama</th>
            <th class="py-3 px-8 text-gray-700 text-left">Kelas</th>
            <th class="py-3 px-8 text-gray-700 text-center">Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="hover:bg-gray-200"
            v-for="(student, index) in students"
            :key="student.id"
          >
            <td class="py-5 px-8 text-gray-900">
              {{ pagination.from + index }}
            </td>
            <td class="py-5 px-8 text-gray-900">{{ student.nama }}</td>
            <td class="py-5 px-8 text-gray-900">{{ student.kelas }}</td>
            <td class="flex justify-around items-center py-5 px-8">
              <button
                class="px-3 py-1 bg-yellow-400 hover:bg-yellow-300 rounded text-gray-50"
                @click="toggleModalEdit(student.id)"
              >
                Edit
              </button>
              <button
                class="px-3 py-1 bg-red-400 hover:bg-red-300 rounded text-gray-50"
                @click="deleteStudent(student.id)"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <ShimmerTable v-else />
    <ModalCreate
      :headerTitle="'Tambah Murid'"
      @closeModal="toggleModalCreate"
      @newStudent="addNewStudent"
      v-if="showModalCreate"
    />
    <ModalEdit
      :headerTitle="'Edit Murid'"
      :studentId="studentId"
      @editStudent="editStudent"
      @closeModal="toggleModalEdit"
      v-if="showModalEdit"
    />
    <div class="flex">
      <div v-for="(link, index) in pagination.links" :key="index">
        <button
          v-if="link.url"
          class="px-4 py-2 border"
          :class="[
            { 'bg-blue-700 text-gray-100': link.active },
            { 'text-gray-600': link.active == false },
          ]"
          :disabled="link.active"
          @click="buttonControl(link.url)"
        >
          <span v-html="link.label"></span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import ShimmerTable from "../components/shimmer/ShimmerTable.vue";
import ModalCreate from "../components/modal/ModalCreate.vue";
import ModalEdit from "../components/modal/ModalEdit.vue";

export default {
  name: "Home",
  data() {
    return {
      students: [],
      student: {},
      studentId: "",
      pagination: {},
      showModalCreate: false,
      showModalEdit: false,
    };
  },
  components: { ShimmerTable, ModalCreate, ModalEdit },
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
        .then((data) => {});
    },
    fetchStudent() {
      fetch("https://sarunk-vue-api.herokuapp.com/api/student")
        .then((res) => res.json())
        .then((data) => {
          console.log(data.message);
          this.students = data.data.data;
          this.pagination = data.data;
        });
    },
    toggleModalCreate() {
      this.showModalCreate = !this.showModalCreate;
    },
    addNewStudent(data) {
      if (data.id) {
        this.students.shift();
        this.students.unshift(data);
        this.students.length >= 5 ? this.students.pop() : "";
      } else {
        this.students.unshift(data);
      }
    },
    toggleModalEdit(id) {
      this.showModalEdit = !this.showModalEdit;
      this.studentId = id;
    },
    editStudent(data) {
      const dataIndex = this.students.findIndex((i) => i.id == data.id);
      this.students.splice(dataIndex, 1, data);
    },
    buttonControl(url) {
      this.students = [];
      fetch(url)
        .then((res) => res.json())
        .then((data) => {
          this.students = data.data.data;
          this.pagination = data.data;
        });
    },
  },
  mounted() {
    this.fetchStudent();
  },
};
</script>