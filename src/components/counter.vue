<script setup>
const props = defineProps({
    count: {
        type: Number,
        required: true
    },
    message: {
        type: String,
        required: true
    },
    sentenceEnd: {
        type: String,
        required: false
    }
})

</script>

<template>
    <div>
        <p>{{ message }} <span :class="color" v-if="count > 0">{{ count }}</span> {{ sentenceEnd }}</p>
    </div>
</template>

