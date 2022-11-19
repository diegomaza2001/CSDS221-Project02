<template>
    <v-dialog
    v-model="dialog"
    max-width="344"
    >
    <template v-slot:activator="{ props }">
        <v-btn
        class="mb-0 mt-2"
        :prepend-icon="this.icon" 
        :variant="btnVariant"
        v-bind="props"
        >
        {{ this.buttonText }}
        </v-btn>
    </template>
    <v-card>
        <v-card-title>
            <v-icon :icon="this.icon" /> {{ this.dialogHeaderText }}
        </v-card-title>
        <v-card-text>
        <v-container>
            <v-row v-if="this.addDialog">
                <v-col>
                    <v-text-field
                    label="Title"
                    v-model="title"
                    required
                    ></v-text-field>
                </v-col>
            </v-row>
            <v-row>
            <v-col>
                <v-text-field
                label="Description"
                v-model="description"
                required
                ></v-text-field>
            </v-col>
            </v-row>
            <v-row>
            <v-col>
                <input type="date" 
                v-model="deadline"
                required>
            </v-col>
            </v-row>
            <v-row>
            <v-col>
                <v-radio-group inline label="Priority" color="primary" v-model="priority">
                    <v-radio label="Low" value="Low"></v-radio>
                    <v-radio label="Med" value="Med"></v-radio>
                    <v-radio label="High" value="High"></v-radio>
                </v-radio-group>
            </v-col>
            </v-row>
        </v-container>
        </v-card-text>
        <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
        prepend-icon="mdi-plus-circle"
        color="primary"
        variant="flat"
        @click="submitClicked"
        >
            {{ this.submitText }}
        </v-btn>
        <v-btn
        prepend-icon="mdi-cancel"
        color="error"
        variant="flat"
        @click="this.dialog = false"
        >
            Cancel
        </v-btn>
        </v-card-actions>
    </v-card>
    </v-dialog>
</template>

<script>
import toastr from "toastr";
import 'toastr/build/toastr.css';
export default {
    props: {
        taskList: Array,
        addDialog: Boolean,
    },
    methods: {
        submitClicked: function () {
            
            if(this.addDialog){
                console.log("reached");
                this.addTask();

            }
            else {
                this.updateTask();
            }
        },
        addTask: function () {
            this.taskList.push({
                title: this.title,
                description: this.description,
                deadline: this.deadline,
                complete: false,
                priority: this.priority,
            });
            this.title = "";
            this.description = "";
            this.deadline = "";
            this.priority = "";
            this.dialog = false;
            toastr.options = {
                "newestOnTop": false,
                "positionClass": "toast-bottom-right",
                "showDuration": "300"
            }   
            toastr["success"]("Task was added successfully!");
        },
        updateTask: function () {
            if(false){

            }
            else{
                for(let i = 0; i < this.taskList.length; i++){
                    if(this.taskList[i].title == this.title){
                        this.taskList[i] = {
                            title: this.title,
                            description: this.description,
                            deadline: this.deadline,
                            priority: this.priority,
                        };
                        this.description = "";
                        this.deadline = "";
                        this.priority = "";
                        this.dialog = false;
                        toastr.options = {
                            "newestOnTop": false,
                            "positionClass": "toast-bottom-right",
                            "showDuration": "300"
                        }   
                        toastr["success"]("Task was updated successfully!");
                    }
                } 
            }
            
        },
    },
    created() {
        if(this.addDialog){
            this.icon = "mdi-plus-circle";
            this.buttonText = "add";
            this.dialogHeaderText = "Add Task";
            this.btnVariant="tonal";
            this.submitText= "Add";
        }
        else {
            this.icon = "mdi-circle-edit-outline";
            this.buttonText = "update";
            this.dialogHeaderText = "Edit Task";
            this.btnVariant="flat";
            this.submitText= "Edit";
        }
        // props are exposed on `this`
        console.log(this.taskList);
    },
    data: () => ({
        title: "",
        description: "",
        deadline: "",
        priority: "",
        icon: "",
        buttonText: "",
        dialogHeaderText: "",
        btnVariant: "",
        submitText: "",
        dialog: false,
    }),
  }
</script>