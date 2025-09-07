<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 p-4">
    <div class="max-w-2xl mx-auto">
       Header
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-gray-800 mb-2">Vue Task Manager</h1>
        <p class="text-gray-600">Demonstrating Vue.js fundamentals with style</p>
      </div>

       Stats Cards
      <div class="grid grid-cols-3 gap-4 mb-8">
        <div class="bg-white rounded-lg p-4 shadow-md text-center">
          <div class="text-2xl font-bold text-blue-600">{{ totalTasks }}</div>
          <div class="text-sm text-gray-600">Total Tasks</div>
        </div>
        <div class="bg-white rounded-lg p-4 shadow-md text-center">
          <div class="text-2xl font-bold text-green-600">{{ completedTasks }}</div>
          <div class="text-sm text-gray-600">Completed</div>
        </div>
        <div class="bg-white rounded-lg p-4 shadow-md text-center">
          <div class="text-2xl font-bold text-orange-600">{{ pendingTasks }}</div>
          <div class="text-sm text-gray-600">Pending</div>
        </div>
      </div>

       Add Task Form
      <div class="bg-white rounded-lg shadow-md p-6 mb-6">
        <h2 class="text-xl font-semibold mb-4">Add New Task</h2>
        <form @submit.prevent="addTask" class="space-y-4">
          <div>
            <input
              v-model="newTask.title"
              type="text"
              placeholder="Task title..."
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none"
              required
            />
          </div>
          <div class="flex gap-4">
            <select
              v-model="newTask.priority"
              class="flex-1 px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none"
            >
              <option value="low">Low Priority</option>
              <option value="medium">Medium Priority</option>
              <option value="high">High Priority</option>
            </select>
            <button
              type="submit"
              class="px-6 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors duration-200 font-medium"
            >
              Add Task
            </button>
          </div>
        </form>
      </div>

       Search and Filter
      <div class="bg-white rounded-lg shadow-md p-4 mb-6">
        <div class="flex gap-4">
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Search tasks..."
            class="flex-1 px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none"
          />
          <select
            v-model="filterStatus"
            class="px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none"
          >
            <option value="all">All Tasks</option>
            <option value="pending">Pending</option>
            <option value="completed">Completed</option>
          </select>
        </div>
      </div>

       Tasks List
      <div class="space-y-3">
        <div
          v-for="task in filteredTasks"
          :key="task.id"
          class="bg-white rounded-lg shadow-md p-4 transition-all duration-200 hover:shadow-lg"
          :class="{
            'opacity-75': task.completed,
            'border-l-4 border-red-500': task.priority === 'high',
            'border-l-4 border-yellow-500': task.priority === 'medium',
            'border-l-4 border-green-500': task.priority === 'low'
          }"
        >
          <div class="flex items-center justify-between">
            <div class="flex items-center space-x-3">
              <input
                type="checkbox"
                v-model="task.completed"
                class="w-5 h-5 text-blue-600 rounded focus:ring-blue-500"
              />
              <div>
                <h3
                  class="font-medium"
                  :class="{
                    'line-through text-gray-500': task.completed,
                    'text-gray-800': !task.completed
                  }"
                >
                  {{ task.title }}
                </h3>
                <div class="flex items-center space-x-2 text-sm text-gray-500">
                  <span
                    class="px-2 py-1 rounded-full text-xs font-medium"
                    :class="{
                      'bg-red-100 text-red-800': task.priority === 'high',
                      'bg-yellow-100 text-yellow-800': task.priority === 'medium',
                      'bg-green-100 text-green-800': task.priority === 'low'
                    }"
                  >
                    {{ task.priority.toUpperCase() }}
                  </span>
                  <span>{{ formatDate(task.createdAt) }}</span>
                </div>
              </div>
            </div>
            <button
              @click="deleteTask(task.id)"
              class="text-red-500 hover:text-red-700 transition-colors duration-200 p-2"
              title="Delete task"
            >
              <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path>
              </svg>
            </button>
          </div>
        </div>

         Empty State
        <div
          v-if="filteredTasks.length === 0"
          class="text-center py-12 text-gray-500"
        >
          <svg class="w-16 h-16 mx-auto mb-4 text-gray-300" fill="currentColor" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M3 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z" clip-rule="evenodd"></path>
          </svg>
          <p class="text-lg font-medium">No tasks found</p>
          <p class="text-sm">{{ searchQuery ? 'Try adjusting your search' : 'Add your first task above' }}</p>
        </div>
      </div>

       Progress Bar
      <div v-if="totalTasks > 0" class="mt-8 bg-white rounded-lg shadow-md p-4">
        <div class="flex justify-between items-center mb-2">
          <span class="text-sm font-medium text-gray-700">Progress</span>
          <span class="text-sm text-gray-500">{{ Math.round(progressPercentage) }}%</span>
        </div>
        <div class="w-full bg-gray-200 rounded-full h-2">
          <div
            class="bg-blue-600 h-2 rounded-full transition-all duration-300"
            :style="{ width: progressPercentage + '%' }"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'

type Task = {
  completed: boolean;
  value: string;
  title: string;
}

// Reactive state
const tasks = []
const searchQuery = ref('')
const filterStatus = ref('all')
const newTask = ref({
  title: '',
  priority: 'medium'
})

// Computed properties
const totalTasks = computed(() => tasks.value.length)
const completedTasks = computed(() => tasks.value.filter(task => task.completed).length)
const pendingTasks = computed(() => tasks.value.filter(task => !task.completed).length)
const progressPercentage = computed(() =>
  totalTasks.value > 0 ? (completedTasks.value / totalTasks.value) * 100 : 0
)

const filteredTasks = computed(() => {
  let filtered = tasks.value

  // Filter by search query
  if (searchQuery.value) {
    filtered = filtered.filter(task =>
      task.title.toLowerCase().includes(searchQuery.value.toLowerCase())
    )
  }

  // Filter by status
  if (filterStatus.value === 'completed') {
    filtered = filtered.filter(task => task.completed)
  } else if (filterStatus.value === 'pending') {
    filtered = filtered.filter(task => !task.completed)
  }

  return filtered
})

// Methods
const addTask = () => {
  if (newTask.value.title.trim()) {
    tasks.value.push({
      id: Date.now(),
      title: newTask.value.title.trim(),
      priority: newTask.value.priority,
      completed: false,
      createdAt: new Date()
    })

    // Reset form
    newTask.value.title = ''
    newTask.value.priority = 'medium'
  }
}

const deleteTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id)
}

const formatDate = (date) => {
  return new Intl.DateTimeFormat('en-US', {
    month: 'short',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit'
  }).format(date)
}

// Lifecycle
onMounted(() => {
  // Add some sample tasks
  tasks.value = [
    {
      id: 1,
      title: 'Learn Vue.js Composition API',
      priority: 'high',
      completed: true,
      createdAt: new Date(Date.now() - 86400000)
    },
    {
      id: 2,
      title: 'Build a portfolio project',
      priority: 'medium',
      completed: false,
      createdAt: new Date(Date.now() - 43200000)
    },
    {
      id: 3,
      title: 'Practice reactive programming',
      priority: 'low',
      completed: false,
      createdAt: new Date()
    }
  ]
})
</script>
