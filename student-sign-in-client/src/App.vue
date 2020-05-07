<template>
  <div id="app">
    <NewStudentForm v-on:student-added = "newStudentAdded"></NewStudentForm> <!--student data originates here-->
    
    <StudentTable
      v-bind:students = "students"
      v-on:student-present = "studentArrivedOrLeft"
      v-on:delete-student = "studentDeleted">
    </StudentTable> <!--respond to events from student table initiated in student row-->
    
    <StudentMessage v-bind:message = "message" v-bind:name = "name"></StudentMessage> <!--display message if student arrives or leaves-->
  </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentTable from './components/StudentTable.vue'
import StudentMessage from './components/StudentMessage.vue'



export default {
  name: 'App',
  components: {
    NewStudentForm,
    StudentTable,
    StudentMessage
  },
  data(){
    return {
      students: [],
      message: '',
      name: '',
      newStudentName: ''
    }
  },
  mounted(){
    this.updateStudents()
  },
  methods: {
    newStudentAdded(student){
      this.$student_api.addStudent(student).then( student => {
        this.updateStudents()
      }).catch( err => {
        let message = err.response.data.join(', ')
        alert('Error adding student.\n', message) //it's not sending the message for some reason
      })
    },
    studentArrivedOrLeft(student){ //display welcome if present, goodbye if not present
      this.$student_api.updateStudent(student).then( () =>{
        this.name = student.name
        this.message = student.present ? 'Welcome, ' : 'Goodbye, '
        this.updateStudents()
      })
    },
    studentDeleted(student){ //remove student from list
      this.$student_api.deleteStudent(student).then( () => {
        this.updateStudents()
      })
    },
    updateStudents(){
      this.$student_api.getAllStudents().then( students => {
        this.students = students
      })
    }
  }
}
</script>

<style>


</style>
