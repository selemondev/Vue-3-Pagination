<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
import { UserType } from "./Types/User.interface";
const userLists = ref<UserType[]>([]);
const currentPage = ref<number>(1);
const perPage = ref<number>(5);
const fetchUsers = async () => {
  const response = await fetch("https://randomuser.me/api/?results=100");
  const data = await response.json();
  data.results.forEach((data: UserType) => {
    userLists.value.push(data);
  });
};

const fetchUsersList = computed(() => {
  const begin = (currentPage.value - 1) * perPage.value;
  const end = currentPage.value * perPage.value;
  const results = userLists.value.slice(begin, end);
  return results;
});

const pages = computed(() => {
  return userLists.value.length / perPage.value;
})


const handlePageChange = (num: number) => {
  return currentPage.value = currentPage.value + num
};

onMounted(() => {
  fetchUsers();
});
</script>
<template>
  <div class="grid-layout min-h-screen">

    <div>
      <h1 class="text-xl font-semibold md:text-3xl">Vue 3 Pagination</h1>
    </div>

    <div class="w-72 md:w-[500px]">
      <div class="flex flex-col">
        <div class="overflow-x-auto sm:-mx-6 lg:-mx-8">
          <div class="py-2 inline-block min-w-full sm:px-6 lg:px-8">
            <div class="overflow-hidden">
              <table class="min-w-full">
                <thead class="border-b">
                  <tr class="bg-gray-200">
                    <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                      Name
                    </th>
                    <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                      Gender
                    </th>
                    <th scope="col" class="text-sm text-center font-medium text-gray-900 px-6 py-4">
                      Email
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr class="bg-white border-b" v-for="user in fetchUsersList" :key="user.cell">
                    <td class="text-sm text-black px-6 py-4 whitespace-nowrap">
                      {{ user.name.first }}
                    </td>
                    <td class="text-sm text-black px-6 py-4 whitespace-nowrap">
                      {{ user.gender }}
                    </td>
                    <td class="text-sm text-black px-6 py-4 whitespace-nowrap">
                      {{ user.email }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>

    </div>

    <div class="flex items-center space-x-4">
      <button type="button" class="py-2 px-4 text-white bg-green-500 rounded-md disabled:bg-green-300"
        :disabled="currentPage === 1" @click="handlePageChange(-1)">Prev</button>
      <span>{{ currentPage }}</span>
      <button type="button" class="py-2 px-4 text-white bg-green-500 rounded-md disabled:bg-green-300"
        :disabled="(currentPage === pages)" @click="handlePageChange(1)">Next</button>
    </div>

  </div>
</template>