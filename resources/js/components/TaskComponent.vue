<template>
    <div class="container">
        <ul class="list-group">
            <li class="list-group-item" v-for="task in tasks" :key="task.id">
                <a href="#">{{ task.name }}</a>
            </li>
        </ul>
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
                .then(response => {
                    this.tasks = response.data
                    console.log(response)
                })
                .catch()
        },

        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
