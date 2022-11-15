<template>
    <v-container>
        <v-row 
        justify="center"
        >
        <v-btn
            v-on:click="deleteTask"
            class="mt-0 mb-2"
            variant="flat"
            color="error"
            prepend-icon="mdi-delete-forever"
            >
                Delete
        </v-btn>
        </v-row>
    </v-container>
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
        toastr["success"]("deletion complete");
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