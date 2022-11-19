<template>
    <v-btn
        v-on:click="deleteTask"
        variant="flat"
        color="error"
        prepend-icon="mdi-delete-forever"
        >
            Delete
    </v-btn>
</template>

<script>
import toastr from "toastr";
import 'toastr/build/toastr.css';
  export default {
    props: {
        taskList: Array,
        title: String
    },
    methods: {
        deleteTask: function (event) {
            for(let i = 0; i < this.taskList.length; i++){
                if(this.taskList[i].title == this.title){
                    this.taskList.splice(i,1);
                }
            }
            toastr.options = {
                "newestOnTop": false,
                "positionClass": "toast-bottom-right",
                "showDuration": "300"
            }   
            toastr["success"]("Task was deleted successfully!");
        },
    },
    created() {
        // props are exposed on `this`
        console.log(this.title);
    },
    data: () => ({
        dialog: false,
    }),
  }
</script>