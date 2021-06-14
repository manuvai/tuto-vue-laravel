<template>
    <div class="container">
        <div class="form-row">
            <div class="col-row">
                 <input type="text" name="" class="form-control" @keyup="searchTask" v-model="q" placeholder="Rechercher une tâche ..">
            </div>
        </div>
        <add-task-component @task-added="refresh"></add-task-component>
        <ul class="list-group">
            <li class="list-group-item d-flex justify-content-between align-center" v-for="task in tasks.data" :key="task.id">
                <a href="#">{{ task.name }}</a>
                <div>
                    <button type="button" class="btn btn-secondary" data-toggle="modal" data-target="#editModal" @click="getTask(task.id)">
                        Editer 
                    </button>
                    <button type="button" class="btn btn-danger" @click="deleteTask(task.id)">
                        Supprimer
                    </button>
                </div>
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
                taskToEdit: '',
                q: ''
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
            deleteTask(id) {
                axios.delete('/tasks/' + id)
                    .then(response => this.tasks = response.data)
                    .catch(error => console.log(error));
            },
            searchTask() {
                if (this.q.length > 3) {
                    axios.get('/taskslist/' + this.q)
                        .then(response => this.tasks = response.data)
                        .catch(error => console.log(error));

                } else {
                    this.getResults();
                }
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
