<script setup>
import { v4 as uuidv4 } from "uuid"
import { ref, reactive, onBeforeMount } from "vue"
import { computed } from "vue"
const todotxt = ref("")
const todotextlist = reactive([])
const addTodo = () => {
  if (todotxt.value !== "") {
    const item = {
      id: uuidv4(),
      title: todotxt.value,
    }
    todotextlist.unshift(item)
    todotxt.value = ""
    console.log(todotextlist)
  }
}
const textsCount = computed(() => todotextlist.length)
const noText = computed(() => textsCount.value === 0)
const remove = (item) => {
    const index = todotextlist.findIndex((todotxt) => {
    return todotxt.id === item.id
  })
  todotextlist.splice(index, 1)
}
const isChecked = ref(false)
</script>

<template>
  <main class="container mx-auto">
    <header class="m-2">
      <h1 class="text-6xl font-thin select-none">TODO!</h1>
      <div class="font-semibold select-none text-neutral-600">simple and studid todo app</div>
    </header>
    <form class="px-10 py-12 bg-white shadow-sm">
      <section class="flex">
        <input
          v-model="todotxt"
          type="text"
          placeholder="做點重要的事吧..."
          class="w-full text-2xl focus:outline-none input-lg input input-bordered"
        />
        <button @click.prevent="addTodo" class="text-xl btn-lg btn btn-neutral">新增</button>
      </section>
    </form>

    <section class="px-10 py-6 mt-4 bg-white">
      <div>
      <h3 v-if="noText">目前無任何 todo 待辦事項</h3>
      <h3 v-else>目前共計 {{ textsCount }} 項事項需處理</h3>
      </div>
      <ul class="">
        <li v-for="item in todotextlist" :todotxt="item" class="flex items-center justify-between px-3 py-3 my-3 bg-slate-200">
          <label class="space-x-3">
            <input type="checkbox" v-model="isChecked"/>
            <span :class="{ completed: isChecked }">{{ item.title }}</span>
            <button class="px-2 py-1 text-white bg-black" @click="remove">X</button>
          </label>
        </li>
      </ul>
    </section>
  </main>
</template>

<style scoped>
.completed {
  @apply line-through text-gray-500;
}
</style>
