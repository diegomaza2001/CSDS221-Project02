<template>
    <v-dialog
    v-model="dialog"
    max-width="344"
    @click:outside="this.closeDialog"
    >
    <template v-slot:activator="{ props }">
        <v-btn
        :prepend-icon="this.icon" 
        :variant="this.btnVariant"
        :color="this.btnColor"
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
                    :error-messages="this.titleErr"
                    @change="this.titleErr=[]"
                    ></v-text-field>
                </v-col>
            </v-row>
            <v-row>
            <v-col>
                <v-text-field
                label="Description"
                v-model="description"
                :error-messages="this.descriptionErr"
                @change="this.descriptionErr=[]"
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
                <v-radio-group inline label="Priority" color="primary" v-model="this.priority">
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
        @click="this.closeDialog"
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
import moment from "moment";
export default {
    props: {
        taskList: Array,
        addDialog: Boolean,
        inputTitle: String,
    },
    methods: {
        closeDialog: function () {
            this.title = "";
            this.description = "";
            this.deadline = "";
            this.priority = "";
            this.dialog = false;
            this.titleErr = [];
            this.descriptionErr = [];
        },
        submitClicked: function () {
            
            if(this.addDialog){
                this.addTask();
            }
            else {
                this.updateTask();
            }
        },
        addTask: function () {
            if(this.validateSubmission()){
                if(this.deadline){
                    let displayDeadline = moment(this.deadline).format("MM/DD/YYYY");
                }
                else {
                    this.displayDeadline = "No deadline selected";
                }
                this.taskList.push({
                    title: this.title,
                    description: this.description,
                    deadline: this.displayDeadline,
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
            } 
        },
        updateTask: function () {
            if(!this.validateSubmission()){
                console.log("Invalid Submission");
            }
            else{
                for(let i = 0; i < this.taskList.length; i++){
                    if(this.taskList[i].title == this.inputTitle){
                        if(this.deadline){
                            let displayDeadline = moment(this.deadline).format("MM/DD/YYYY");
                        }
                        else {
                            this.displayDeadline = "No deadline selected";
                        }
                        this.taskList[i] = {
                            title: this.taskList[i].title,
                            description: this.description,
                            deadline: this.displayDeadline,
                            priority: this.priority,
                        };
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
        validateSubmission: function () {
            let returnVal = true;
            if(this.addDialog){
                if(this.title==""){
                    this.titleErr=[];
                    this.titleErr.push("Title cannot be empty");
                    returnVal = false;
                }
                else{
                    for(let i = 0; i < this.taskList.length; i++){
                        if(this.taskList[i].title == this.title){
                            this.titleErr=[];
                            this.titleErr.push("Title must be unique");
                            returnVal = false;
                        }
                    } 
                }
            }
            if(this.description==""){
                this.descriptionErr=[];
                this.descriptionErr.push("Description cannot be empty");
                returnVal = false;

            }
            return returnVal;
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
            this.btnColor="primary";
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
        btnColor: "",
        submitText: "",
        titleErr: [],
        descriptionErr: [],
        dialog: false,
    }),
  }
</script>