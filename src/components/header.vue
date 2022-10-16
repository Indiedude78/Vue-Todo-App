<script>
export default {
    name: "Header",
    props: {
        title: {
            type: String,
            default: "Demo App"
        }
    },
    methods: {
        titleToUpperCase() {
            return this.title.toUpperCase();
        }
    }
}
</script>

<template>
    <header class="p-2 m-auto">
        <h1 class="text-white text-4xl underline mb-1">{{titleToUpperCase()}}</h1>
    </header>
</template>

<style scoped>
header {
    line-height: 1.5;
    width: max-content;
    height: fit-content;
    margin: auto;
}


h1 {
    text-align: center;
    margin: auto;
    padding: auto;
}
</style>
