<template>
    <div :key="task.id" v-for="task in tasks">
        <!-- like 2: v-for loops through each task in tasks -->
        <Task @delete-task="$emit('delete-task', task.id)" :task="task" />
    </div>
</template>

<script>
import Task from "./Task.vue";

export default {
    name: "Tasks", // name of this component
    props: { // this component "Tasks" will take in "tasks" Array as prop
        tasks: Array
    },
    components: { Task }, // need to register the Task component inside this "Tasks" component
    emits: ['delete-task'],
}
</script>