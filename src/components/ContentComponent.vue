<template>
  <AddComponent @addStudent="addStudent1"></AddComponent>
  <table class="table table-striped table-bordered text-center">
    <thead>
      <tr>
        <th>#</th>
        <th>Name</th>
        <th>City</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="student in students" :key="student.id">
        <td>{{ student.id }}</td>
        <td>{{ student.name }}</td>
        <td>{{ student.city }}</td>
        <td>
          <i class="fa fa-trash" @click="deleteStudent(student.id)"></i> |
          <i
            class="fas fa-trash fa-fw"
            @click="fillmember(student)"
            data-bs-toggle="modal"
            data-bs-target="#exampleModal1"
          ></i>
        </td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <th colspan="3">Total Number of Students : {{ students.length }}</th>
      </tr>
    </tfoot>
  </table>
  <!-- Modal -->
  <div
    class="modal fade"
    id="exampleModal1"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <div class="form-group">
            <label for="name">Name</label>
            <input
              type="text"
              class="form-control"
              id="name"
              v-model="stdname"
            />
          </div>
          <div class="form-group">
            <label for="city">City</label>
            <input
              type="text"
              class="form-control"
              id="city"
              v-model="stdcity"
            />
          </div>
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            Close
          </button>
          <button
            type="button"
            class="btn btn-primary"
            @click="editStudent(student)"
          >
            Update
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import AddComponent from "./AddComponent.vue";
export default {
  name: "ContentComponent",
  data() {
    return {
      students: [],
      stdname: "",
      stdcity: "",
    };
  },
  async created() {
    this.students = await axios
      .get("http://localhost:5000/students")
      .then((res) => res.data);
  },
  comopnents: {
    AddComponent,
  },
  methods: {
    async addStudent1(data) {
      console.log("done");

      await axios
        .post("http://localhost:5000/students", {
          id: this.students.length + 1,
          ...data,
        })
        .then((res) => {
          this.students.push(res.data);
        });
    },
    async deleteStudent(id) {
      await axios.delete("http://localhost:5000/students/" + id).then(() => {
        this.students = this.students.filter((student) => student.id !== id);
      });
    },
    fillmember(student) {
      this.stdid = student.id;
      this.stdname = student.name;
      this.stdcity = student.city;
    },
    async editStudent() {
      // console.log(std)
      const student = this.students.find(
        (student) => student.id === this.stdid
      );
      const updatedObj = {
        id: this.stdid,
        name: this.stdname,
        city: this.stdcity,
      };
      this.fillmember(student);
      await axios
        .put("http://localhost:5000/students/" + this.stdid, updatedObj)
        .then((res) => {
          console.log("done");
          console.log(res.data);
          this.students = this.students.map((student) =>
            student.id == this.stdid ? res.data : student
          );
        });
    },
  },
  components: { AddComponent },
};
</script>
