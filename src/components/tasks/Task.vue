<template>
        <li class="list-group-item py-3">
            <div
                class="d-flex justify-content-start align-items-center"
            >
                <input
                    class="form-check-input mt-0" :class="completedClass" 
                    type="checkbox" :checked="task.is_completed"
                />
                <div
                    class="ms-2 flex-grow-1" 
                    :class="completedClass" 
                    title="Double click the text to edit or remove" 
                    @dblclick="$event => isEdit = true"
                >
                    <div class="relative" v-if="isEdit">
                        <input 
                            type="text"
                            class="editable-task"
                            v-focus
                            @keyup.esc="undo" 
                            @keyup.enter="updateTask" 
                            v-model="editingTask"
                        />
                    </div>
                    <span v-else>{{ task.name }}</span>
                </div>
                <!-- <div class="task-date">24 Feb 12:00</div> -->
            </div>
            <TaskActions @edit="$event => isEdit = true" v-show="!isEdit"/>
        </li>
</template>
    
<script setup>
import { computed, ref } from 'vue';
import TaskActions from './TaskActions.vue';

const props = defineProps({
    task: Object
})

const emit = defineEmits(['updated'])

const isEdit = ref(false);

const editingTask = ref(props.task.name)

const completedClass = computed(() => props.task.is_completed ? "completed" : "")

// custom directive to apply focus to input box
const vFocus = {
    mounted: (el) => el.focus()
}

const updateTask = (event) => {
    const updatedTask = { ...props.task, name: event.target.value };
    isEdit.value = false;
    // emit the payload to parent component(s), event name, payload
    // parent component(s) that have updated custom event named 'updated' will get the updatedTask payload
    emit('updated', updatedTask)
}

const undo = () => {
    isEdit.value = false
    editingTask.value = props.task.name
}

</script>
    
<style>
    
</style>