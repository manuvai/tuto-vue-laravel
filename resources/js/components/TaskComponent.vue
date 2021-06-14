<template>
    <div class="container">
        <add-task-component @task-added="refresh"></add-task-component>
        <ul class="list-group">
            <li class="list-group-item d-flex justify-content-between align-center" v-for="task in tasks.data" :key="task.id">
                <a href="#">{{ task.name }}</a>
                <button type="button" class="btn btn-secondary" data-toggle="modal" data-target="#editModal" @click="getTask(task.id)">
                   Editer 
                </button>
            </li>
            <edit-task-component 
                @task-updated="refresh"
                v-bind:taskToEdit="taskToEdit"/>
        </ul>
        <pagination :data="tasks" @pagination-change-page="getResults" class="mt-5"></pagination>
    </div> 
</template>

<script>
import AddTaskComponent from './AddTaskComponent.vue';
import EditTaskComponent from './EditTaskComponent.vue';
    export default {
  components: { AddTaskComponent, EditTaskComponent },
        data() {
            return {
                tasks: {},
                taskToEdit: ''
            }
        },
        created() {
            axios.get('/taskslist')
                .then(response => this.tasks = response.data)
                .catch(error => console.log(error))
        },
        methods: {
            getResults(page = 1) {
                axios.get('/taskslist?page=' + page)
                    .then(response => {
                        this.tasks = response.data;
                    });
            },
            getTask(id) {
                axios.get('/tasks/edit/' + id)
                    .then(response => this.taskToEdit = response.data)
                    .catch(error => console.log(error));
            },
            refresh(tasks) {
                this.tasks = tasks.data
            }
        },
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
