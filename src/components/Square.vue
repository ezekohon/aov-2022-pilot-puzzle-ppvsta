<template>
    <div class="border-solid border-green border-2 w-20 h-20 flex items-center justify-center"
        :class="{ 'cursor-pointer': value === '' && !isGameFinished }" @click="handleClick(value)">
        {{ value }}
    </div>
</template>

<script setup>
const props = defineProps({
    value: String,
    isPlayer1Turn: Boolean,
    isGameFinished: Boolean
})
const emit = defineEmits(['onClick'])

const handleClick = (value) => {
    if (value === '' && !props.isGameFinished) {
        emit('onClick')
    }
}
</script>

<style lang="scss" scoped>

</style>