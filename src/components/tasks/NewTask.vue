<template>
        <div class="relative">
            <input
                type="text" 
                class="form-control form-control-lg padding-right-lg" 
                placeholder="+ Add new task. Press enter to save." 
                @keydown.enter="addNewTask"
            />
        </div>
</template>
    
<script setup>

import { reactive } from 'vue';

const newTask = reactive({
    name: '',
    is_completed: false
})

// Hit enter key event

const emit = defineEmits(['added'])

const addNewTask = event => {
    // check if event target is not empty
    if (event.target.value.trim()) {
        newTask.name = event.target.value
        event.target.value = ""
        emit('added', newTask)
    }
}

</script>