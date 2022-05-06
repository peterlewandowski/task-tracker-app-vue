<template>
    <AddTask v-show="showAddTask" @add-task="addTask" />
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
    <!-- line 4 does: v-bind tasks prop to tasks data -->
</template>

<script>
import Tasks from "../components/Tasks.vue"
import AddTask from "../components/AddTask.vue"
export default {
    name: 'Home',
    props: {
        showAddTask: Boolean,
    },
    components: {
        Tasks,
        AddTask
    },
    data() {
        return {
            tasks: [],
        }
    },
    methods: {
        async addTask(task) {
            const res = await fetch('api/tasks', {
                method: 'POST',
                headers: {
                    'Content-type': 'application/json',
                },
                body: JSON.stringify(task),
            })

            const data = await res.json()

            this.tasks = [...this.tasks, data]
        },
        async deleteTask(id) {
            if (confirm('Are you sure?')) {
                const res = await fetch(`api/tasks/${id}`, {
                    method: 'DELETE',
                })

                res.status === 200
                    ? (this.tasks = this.tasks.filter((task) => task.id !== id)) /* 'reset' tasks to tasks w/o deletedTask of 'id' */
                    : alert('Error deleting task')
            }
        },
        async toggleReminder(id) {
            const taskToToggle = await this.fetchTask(id)
            const updateTask = { ...taskToToggle, reminder: !taskToToggle.reminder }

            const res = await fetch(`api/tasks/${id}`, {
                method: 'PUT',
                headers: {
                    'Content-type': 'application/json'
                },
                body: JSON.stringify(updateTask)
            })

            const data = await res.json()

            this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: data.reminder } : task)
            /* We're updating a task by changing the 'reminder' value from either true to false, or false to true. */
            /* We want to map through tasks and return an array of what we want (updated tasks) */
            /* For each task, we want to check if task.id is equal to id, if it is, return an array of objects where... */
            /* we have the initial task properties (spread across the initial task), then change the initial reminder to... */
            /* the opposite of the current task reminder. Else, if it doesn't match the id, just return the initial task  */
        },
        async fetchTasks() {
            const res = await fetch('api/tasks')

            const data = await res.json()

            return data
        },
        async fetchTask(id) {
            const res = await fetch(`api/tasks/${id}`)

            const data = await res.json()

            return data
        }
    },
    async created() {
        this.tasks = await this.fetchTasks()
    }
}
</script>