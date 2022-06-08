<template>
  <button type="button" class="btn btn-success w-100 my-1" data-bs-toggle="modal" data-bs-target="#exampleModal">
    Add
  </button>
  <!-- Modal -->
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <div class="form-group">
            <label for="name">Name</label>
            <input type="text" class="form-control" id="name" v-model="newStudent.name">
          </div>
          <div class="form-group">
            <label for="city">City</label>
            <input type="text" class="form-control" id="city" v-model="newStudent.city">
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary" @click="raiseevent">Add</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddComponent',
  data() {
    return {
      newStudent: {
        name: '',
        city: ''
      }
    }
  },
  methods: {
    raiseevent() {
      this.$emit('addStudent', this.newStudent)
      this.newStudent = {
        name: '',
        city: ''
      }
    }
  }
}
</script>
