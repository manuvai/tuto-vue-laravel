<template>
    <div class="container">
        <ul class="list-group">
            <li class="list-group-item" v-for="task in tasks.data" :key="task.id">
                <a href="#">{{ task.name }}</a>
            </li>
        </ul>
        <pagination :data="tasks" @pagination-change-page="getResults" class="mt-5"></pagination>
    </div> 
</template>

<script>
    export default {
        data() {
            return {
                tasks: {}
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
            }
        },
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
