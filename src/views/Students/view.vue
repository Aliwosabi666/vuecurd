<template>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h4>Students</h4>
        <RouterLink  to="/students/create" class="btn btn-primary float-end">Add Employees</RouterLink>
      </div>
      <div class="card-body">
        <h5 class="card-title">Card title</h5>

        <table class="table table-bordered">
          <thead>
          <tr>
            <th scope="col">ID</th>
            <th scope="col">Name</th>
            <th scope="col">Course</th>
            <th scope="col">Email</th>
            <th scope="col">Phone</th>
            <th scope="col">Created At </th>
            <th scope="col">Action </th>
          </tr>
          </thead>
          <tbody v-if="this.students.length > 0">
          <tr v-for="(student, index) in this.students" :key="index">
            <th scope="row" >{{student.id}}</th>

            <td>{{student.name}}</td>
            <td>{{student.course}}</td>
            <td>@{{student.email}}</td>
            <td>@{{student.phone}}</td>
            <td>{{student.created_at}}</td>
            <td>

              <RouterLink  :to="{path : '/stduents/'+student.id+'/edit'}" class="btn btn-warning float-end">Edit</RouterLink>
              <button type="button" @click="deleteStudent(student.id)" class="btn btn-danger float-end">
                Delete
              </button>
            </td>
          </tr>
          </tbody>

          <tbody v-else>
          <tr>
            <td colspan="7">Loading ...</td>

          </tr>
          </tbody>
        </table>



      </div>


    </div>

  </div>
</template>
<script>
import axios from "axios";

export default {
  name: 'students',
  data(){
    return{
      students: []
    }
  },
  mounted() {
    // console.log('I am here')
    this.getStudents();
  },
  methods:{
    getStudents(){
      axios.get('http://127.0.0.1:8000/api/students').then((res) => {
        // console.log(res);
        this.students = res.data.students
        // console.log(this.students);
      });
    },
    deleteStudent(studentId){
      // console.log(studentId);
      if(confirm('Are you sure , you want to delete this data?')){
        // console.log(studentId);
        axios.delete(`http://localhost:8000/api/students/${studentId}/delete`)
            .then((res)=>{
              alert(res.data.message);
              this.getStudents();

        })
            .catch(function (error) {
              if (error.response) {

                if(error.response.status === 404){
                  alert(error.response.data.message);
                }
              }

            });
      }

    }

  },
}
</script>


