<script setup>
import axios from "axios";
import { onMounted, reactive } from "vue";
import { useToast } from "vue-toastification";

import { useRouter } from "vue-router";

import Button from "@/components/Button.vue";
import Input from "@/components/Input.vue";

const router = useRouter();

const initialForm = {
  email: "",
  password: "",
};

const form = reactive(initialForm);

const toast = useToast();

onMounted(() => {
  const token = localStorage.getItem("access_token");

  if (token) {
    router.push("/dashboard");
  }
});

async function handleLogin() {
  const data = {
    email: form.emailAddress,
    password: form.password,
  };

  try {
    const response = await axios.post(
      "http://192.168.0.157:8000/v1/login",
      data
    );
    const token = response.data.access_token;
    localStorage.setItem("access_token", token);
    toast.success("Login Successfull");
    Object.keys(form).forEach((key) => {
      form[key] = "";
    });
    router.push("/dashboard");
  } catch (err) {
    toast.error("An error occured");
    console.log(err);
  }
}
</script>

<template>
  <main
    class="flex flex-col justify-center items-center w-[30rem] bg-white rounded-xl px-10 py-6"
  >
    <img src="/login.png" class="pb-4" />
    <div class="flex flex-col items-start py-4 gap-4">
      <Input v-model="form.emailAddress" label="Email Address" type="email" />
      <Input v-model="form.password" label="Password" type="password" />
    </div>
    <Button @click="handleLogin" className="w-full">Login</Button>
  </main>
</template>
