<script setup>
import axios from "axios";
import { onMounted, reactive } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

import Button from "@/components/Button.vue";

const state = reactive({
  messages: [],
  isLoading: true,
});

onMounted(async () => {
  const token = localStorage.getItem("access_token");

  if (!token) {
    router.push("/login");
  }

  try {
    const response = await axios.get("http://192.168.0.157:8000/v1/contact", {
      headers: { Authorization: `Bearer ${token}` },
    });
    state.messages = response.data;
  } catch (error) {
    console.error("Error fetching messsages", error);
  } finally {
    state.isLoading = false;
  }
});

const handleLogout = () => {
  localStorage.removeItem("access_token");
  router.push("/login");
};
</script>

<template>
  <main class="w-full h-screen">
    <nav
      class="w-full flex justify-between items-center py-4 px-10 border-b border-gray-100 shadow-md"
    >
      <h1 class="text-xl font-bold">Contact Form Dashboard</h1>
      <Button @click="handleLogout">Logout</Button>
    </nav>

    <div class="px-6 pt-8 pb-20 h-[90vh] overflow-scroll">
      <div v-if="state.isLoading">loading messages...</div>
      <ul class="flex flex-wrap gap-10">
        <li v-for="each in state.messages" :key="each.email">
          <div
            class="flex flex-col gap-1 w-72 px-6 py-4 bg-white border shadow-md"
          >
            <span class="font-semibold"
              >{{ each.firstname }}&nbsp;{{ each.lastname }}</span
            >
            <span class="text-sm text-gray-600">{{ each.email }}</span>
            <div>
              <p class="line-clamp-3">
                {{ each.message }}
              </p>
              <span class="text-blue-700 underline text-sm cursor-pointer"
                >read more...</span
              >
            </div>
          </div>
        </li>
      </ul>
    </div>
  </main>
</template>
