<template>
    <v-container>
        <v-row 
        justify="end">
            <v-dialog
            v-model="dialog"
            max-width="344"
            >
            <template v-slot:activator="{ props }">
                <v-btn
                class="mb-0 mt-2"
                prepend-icon="mdi-plus-circle" 
                variant="tonal"
                v-bind="props"
                >
                Add
                </v-btn>
            </template>
            <v-card>
                <v-card-title>
                    <v-icon icon="mdi-plus-circle" /> Add Task
                </v-card-title>
                <v-card-text>
                <v-container>
                    <v-row>
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
                @click="addTask"
                >
                    Add
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
        </v-row>
    </v-container>
</template>

<script>
  export default {
    props: {
        taskList: Array,
    },
    methods: {
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
        },
    },
    created() {
        // props are exposed on `this`
        console.log(this.taskList);
    },
    data: () => ({
        title: "",
        description: "",
        deadline: "",
        priority: "",
        dialog: false,
    }),
  }
</script>