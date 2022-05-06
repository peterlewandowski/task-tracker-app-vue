<template>
    <header>
        <h1>{{ title }}</h1>
        <Button v-show="homePage" @btn-click="$emit('toggle-add-task')" :text="showAddTask ? 'Hide' : 'AddTask'" :color="showAddTask ? 'red' : 'green'" />
        <!-- catching btn-click from Button, $emit up to App.vue, conditionally render Button text and color -->
    </header>
</template>

<script>
import Button from "./Button.vue";
export default {
    name: "Header",
    props: {
        title: String,
        showAddTask: Boolean /* receiving showAddTask as props from App.vue */
    },
    components: { Button },
    computed: {
        homePage() {
            if(this.$route.path === '/') {
                return true
            } else {
                return false
            }
        }
    }
}
</script>

<style scoped>
header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}
</style>