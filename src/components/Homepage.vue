<template>
    <v-app-bar color="blue-darken-3">
        <v-container>
            <v-row>
                <v-col cols="6">
                    <v-row>
                      <v-col class="text-end">
                        <v-icon icon="mdi-menu" /> FRAMEWORKS
                      </v-col>
                    </v-row>
                </v-col>
                <v-col cols="6">
                    <AddButton :taskList="taskList"></AddButton>
                </v-col>
            </v-row>
        </v-container>
    </v-app-bar>
    <v-main>
        <v-container>
            <v-row>
                <v-col cols="12">
                    <v-table fixed-header>
                        <thead>
                        <tr>
                            <th class="text-center">
                            Title
                            </th>
                            <th class="text-center">
                            Description
                            </th>
                            <th class="text-center">
                            Deadline
                            </th>
                            <th class="text-center">
                            Priority
                            </th>
                            <th class="text-center">
                            Is Complete
                            </th>
                            <th class="text-center">
                            Action
                            </th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr
                            v-for="task in taskList"
                            :key="task.title"
                        >
                            <td class="text-center">{{ task.title }}</td>
                            <td class="text-center">{{ task.description }}</td>
                            <td class="text-center">{{ task.deadline }}</td>
                            <td class="text-center">{{ task.priority }}</td>
                            <td><v-checkbox color="primary"></v-checkbox></td>
                            <td>
                                <UpdateBtn :taskList="taskList" :title="task.title"></UpdateBtn>
                                <DeleteBtn @delete-event="showSnackbar" :taskList="taskList" :title="task.title"></DeleteBtn> 
                            </td>
                        </tr>
                        </tbody>
                    </v-table>
                </v-col>
            </v-row>
        </v-container>
        <v-snackbar
          v-model="snackbar"
          :timeout="timeout"
        >
          {{ text }}

          <template v-slot:actions>
            <v-btn
              color="blue"
              variant="text"
              @click="snackbar = false"
            >
              Close
            </v-btn>
          </template>
        </v-snackbar>
    </v-main>
</template>
  
<script setup>
    //
    import UpdateBtn from '@/components/UpdateButton.vue';
    import DeleteBtn from '@/components/DeleteButton.vue';
    import AddButton from '@/components/AddButton.vue';
</script>
<script>
import toastr from "toastr";
  export default {
    components: {
      UpdateBtn,
      DeleteBtn,
      AddButton
    },
    created() {
      toastr.success('abc')
    },
    methods: {
      showSnackbar(){
        alert("SHOW SNACKBAR SHOW");
        this.snackbar = true;
      }
    },
    data () {
      return {
        text: 'My timeout is set to 2000.',
        timeout: 2000,
        snackbar: false,
        taskList: [
          {
            title: 'Math Homework 1',
            description: 'write math',
            deadline: '11-14-22',
            complete: false,
            priority: 'med',
          },
          {
            title: 'Math Homework 2',
            description: 'write math',
            deadline: '11-14-22',
            complete: false,
            priority: 'med',
          },
          {
            title: 'Math Homework 3',
            description: 'write math',
            deadline: '11-14-22',
            complete: false,
            priority: 'med',
          },
          {
            title: 'Math Homework 4',
            description: 'write math',
            deadline: '11-14-22',
            complete: false,
            priority: 'med',
          },
        ],
      }
    },
    methods: {

    }
  }
</script>
  