<template>

    
    <tr class="student-row" v-bind:class="'present-' + student.present">
        <td>{{ student.name }}</td>
        <td>{{ student.starID }}</td>
        <td> <input type="checkbox" v-model="student.present" v-on:change="checked(student)"></td> <!--if checked, send student to parent-->
        <td v-show = "edit"> <!--if edit is checked, display delete option-->
            <img class = "delete-icon" v-on:click = "deleteStudent(student)" src = "@/assets/delete.png">
        </td>
    </tr>

</template>

<script>

export default {
    name: 'StudentRow',
    props: {
        student: Object,
        edit: Boolean
    },
    methods: {
        checked(student){
            this.$emit('student-present', student)
        },
        deleteStudent(student){
            if (confirm(`Delete ${student.name}? Are you sure?`)){ //confirm deletion with user
                this.$emit('delete-student', student) //send response to parent
            }
        }
    }
}

</script>

<style>

.present-true {
    color: gray;
    font-style: italic;
}

.present-false {
    font-weight: bold;
}

.delete-icon {
    height: 30px;
}

</style>