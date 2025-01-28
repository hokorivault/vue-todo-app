<template>
        <main style="min-height: 50vh; margin-top: 2rem">
        <div class="container">
            <div class="row">
                <div class="col-md-8 offset-md-2">
                    <!-- Add new Task -->
                    <div class="relative">
                        <input
                            type="text"
                            class="form-control form-control-lg padding-right-lg"
                            placeholder="+ Add new task. Press enter to save."
                        />
                    </div>
                    <!-- List of tasks -->
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
                    <Tasks
                      :show="completedTasksIsVisible && showCompletedTasks"
                      :tasks="completedTasks"
                    />
                </div>
            </div>
        </div>
    </main>
</template>
    
<script setup>
import { ref, onMounted, computed } from "vue";
import { allTasks } from "../http/task-api";
import Tasks from "@/components/tasks/Tasks.vue";

const tasks = ref([]);

onMounted(async() => {
    const { data } = await allTasks();
    tasks.value = data.data;
});

const showCompletedTasks = ref(false);

const uncompletedTasks = computed(() =>
    tasks.value.filter(task => !task.is_completed),
);

const completedTasks = computed(() =>
    tasks.value.filter(task => task.is_completed),
);

const showToggleCompletedBtn = computed(() => 
    uncompletedTasks.value.length > 0 && completedTasks.value.length > 0,
);

const completedTasksIsVisible = computed(() =>
    uncompletedTasks.value.length === 0 || completedTasks.value.length > 0,
);

</script>