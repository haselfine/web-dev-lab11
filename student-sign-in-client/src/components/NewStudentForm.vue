<template>
    <div>

        <div class="alert alert-danger" v-show="errors.length">
            <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
        </div>

        <div class="card add-student m-2 p-2">
                <h4 class="card-title">Add new student</h4>

                <div class="form-group">
                    <label for="name">Name</label>
                    <input id="name" class="form-control" v-model.trim="newStudentName"> <!--attach to v-model-->
                </div>
                <div class="form-group">
                    <label for="starID">Star ID</label>
                    <input id="starID" class="form-control" v-model.trim="newStarID"> <!--attach to v-model-->
                </div>
                <button class="btn btn-primary" v-on:click="addStudent">Add</button>
        </div>

    </div>
</template>

<script>

export default {
    name: 'NewStudentForm',
    data(){
        return {
            newStudentName: '',
            newStarID: '',
            errors: []
        }
    },
    methods: {
        addStudent(){
            this.errors = []
                    
            if(!this.newStudentName) {
                this.errors.push('Name is required.') //if no name, add error to error list
            }
            if(!this.newStarID){
                this.errors.push('StarId is required') //if no starId, add error to error list
            }
            if (this.errors.length == 0) { //if no errors, proceed
                this.newStudentName = this.newStudentName.toLowerCase() //make entire string lowercase
                this.newStudentName = this.newStudentName.charAt(0).toUpperCase() + this.newStudentName.slice(1) //capitalize first letter, concatenate rest of name
                let student = { name: this.newStudentName, starID: this.newStarID, present: false }
                this.$emit('student-added', student)
                this.newStudentName = ''
                this.newStarID = ''
            }
        }
    }
}

</script>

<style>
</style>