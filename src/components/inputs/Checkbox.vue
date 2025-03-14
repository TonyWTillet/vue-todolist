<template>
   <label>
        <input type="checkbox" @change="onChange" v-model="model">
       {{ label }}
    </label>
</template>

<script setup>

const model = defineModel({
    default: false,
    required: true,
    type: Boolean
});

const props = defineProps({
    label: {
        type: String,
        required: true
    }
}) 

const emits = defineEmits(['check', 'uncheck']);

const onChange = (event) => {
    if(event.currentTarget.checked) {
        emits('check', event.currentTarget);
    } else {
        emits('uncheck', event.currentTarget);
    }
}
</script>