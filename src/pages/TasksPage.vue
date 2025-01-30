<template>
        <main style="min-height: 50vh; margin-top: 2rem">
        <div class="container">
            <div class="row">
                <div class="col-md-8 offset-md-2">
                    <!-- Add new Task -->
                    <NewTask />

                    <!-- List of uncompleted tasks -->
                    <Tasks :tasks="uncompletedTasks" />
                    
                    <!-- show toggle button -->
                     <div class="text-center my-3" v-show="showToggleCompletedBtn">
                        <button
                          @click="showCompletedTasks = !showCompletedTasks"
                          class="btn btn-sm btn-secondary"
                        >
                            <span v-if="!showCompletedTasks">Show completed</span>
                            <span v-else>Hide completed</span>
                        </button>
                     </div>
                    
                    <!-- list of completed tasks -->
                    <Tasks :show="completedTasksIsVisible && showCompletedTasks" :tasks="completedTasks" />
                </div>
            </div>
        </div>
    </main>
</template>
    
<script setup>
import { ref, onMounted, computed } from "vue";
import { storeToRefs } from "pinia";
import { useTaskStore } from "@/stores/task";
import Tasks from "@/components/tasks/Tasks.vue";
import NewTask from "@/components/tasks/NewTask.vue";

const store = useTaskStore();
const { completedTasks, uncompletedTasks } = storeToRefs(store)
const { fetchAllTasks } = store

onMounted(async() => {
    await fetchAllTasks()
});

const showCompletedTasks = ref(false);

const showToggleCompletedBtn = computed(() => 
    uncompletedTasks.value.length > 0 && completedTasks.value.length > 0,
);

const completedTasksIsVisible = computed(() =>
    uncompletedTasks.value.length === 0 || completedTasks.value.length > 0,
);

</script>