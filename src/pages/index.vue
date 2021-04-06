<template>
    <div>{{v}}</div>
</template>

<script>
import { ref } from 'vue'
export default {
    setup() {
        const v = ref('这是一个字符串')
        return {v}
    },
}
</script>