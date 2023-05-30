<script setup>
import { ref, onMounted } from 'vue'
import FileIcon from '../assets/Icons/FileIcon.vue'
import PlusIcon from '../assets/Icons/PlusIcon.vue';
import DeleteIcon from '../assets/Icons/DeleteIcon.vue';
import CheckedIcon from '../assets/Icons/CheckedIcon.vue'
import CheckedBackgroundIcon from '../assets/Icons/CheckedBackgroundIcon.vue'

const input = ref('')
const alert = ref(false)
const taskLists = ref([])

const setlocalStorage = (val) => {
    localStorage.setItem("taskLists", JSON.stringify(val))
}

onMounted(() => {
    if (JSON.parse(localStorage.getItem("taskLists")))
        taskLists.value = JSON.parse(localStorage.getItem("taskLists"))
})

const alertFunction = () => {
    alert.value = true
    setTimeout(() => {
        alert.value = false
    }, 2000)
}

const deleteTask = (id) => {
    taskLists.value.splice(id, 1)
    setlocalStorage(taskLists.value)
}

const editStatus = (id) => {
    taskLists.value[id].check = !taskLists.value[id].check
    setlocalStorage(taskLists.value)
}

const enter = () => {
    if (event.key == 'Enter') {
        add()
    }
}

const add = () => {
    if (!input.value) {
        alertFunction()
        return false
    }
    taskLists.value.push({ 'task': input.value, check: false })
    setlocalStorage(taskLists.value)
    input.value = ''
}

</script>

<template>
    <div class="mb-20 my-10">
        <div class="flex items-center justify-between gap-x-5">
            <div class="flex-1">
                <label for="inp" class="flex items-center gap-x-5 border p-3 rounded-lg">
                    <button class="cursor-pointer">
                        <FileIcon class="w-8 h-8" />
                    </button>
                    <input id="inp" type="text" placeholder="What needs to be done?" v-model="input" @keypress="enter()"
                        class="outline-0 w-full" />
                </label>
            </div>
            <button @click="add()"
                class="flex justify-center items-center w-96 p-3 text-xl cursor-pointer bg-[#1890ff]	rounded-lg text-white">
                <PlusIcon class="w-8 h-8 mr-3" />
                Add
            </button>
        </div>
        <p class="text-rose-500 absolute -top-10 right-20 duration-500" :class="{ 'top-20': alert }">Iltimos
            maydonni to'ldiring</p>

        <div class="p-10 border relative rounded-lg mt-10">
            <h2 class="text-3xl font-bold pb-20">Todo List</h2>
            <div class="absolute h-0.5 bg-gray-300 bottom-0 left-0 right-0 top-24"></div>

            <div v-for="(task, id) in taskLists" :key="id" class="flex items-center border-b py-5">
                <button @click="editStatus(id)">
                    <button class="border p-3 rounded" v-if="!task.check"></button>
                    <CheckedBackgroundIcon v-if="task.check" class="w-8 h-8" />
                </button>
                <button :class="{ 'border mx-4 p-2': task.check }">
                    <button v-if="!task.check"
                        class="border border-rose-200 py-1 px-3 mx-5 rounded bg-rose-100 text-rose-500">-</button>
                    <CheckedIcon v-if="task.check" class="w-4 h-4" />
                </button>
                <p :class="{ 'line-through': task.check }">{{ task.task }}</p>
                <button class="ml-auto bg-gray-100 border border-gray-300 py-3 px-6 rounded-lg" @click="deleteTask(id)">
                    <DeleteIcon class="w-6 h-6" />
                </button>
            </div>
        </div>
    </div>
</template>
