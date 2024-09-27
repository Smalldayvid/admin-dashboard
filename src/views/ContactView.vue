<script setup>
import axios from "axios";
import { reactive } from "vue";
import { useToast } from "vue-toastification";

import Button from "@/components/Button.vue";
import Input from "@/components/Input.vue";
import Message from "@/components/Message.vue";

const initialForm = {
  firstName: "",
  lastName: "",
  email: "",
  message: "",
};

const form = reactive(initialForm);

const toast = useToast();

const handleSubmit = async () => {
  const data = {
    firstname: form.firstName,
    lastname: form.lastName,
    email: form.email,
    message: form.message,
  };

  try {
    await axios.post("http://192.168.0.157:8000/v1/contact", data);
  } catch (err) {
    toast.error("An error occured");
    console.log(err);
  } finally {
    toast.success("Successfull");
    Object.keys(form).forEach((key) => {
      form[key] = "";
    });
  }
};
</script>

<template>
  <main class="flex flex-col items-center justify-center h-screen bg-blue-200">
    <div class="bg-white rounded-lg shadow-lg p-5">
      <h1
        class="flex justify-center select-none pb-4 font-extrabold italic text-2xl tracking-wider"
      >
        CONTACT FORM
      </h1>
      <section class="pb-4">
        <div class="flex flex-col gap-2 pb-4">
          <Input v-model="form.firstName" label="First Name" />
          <Input v-model="form.lastName" label="Last Name" />
        </div>
        <Input v-model="form.email" label="Email" type="email" />
      </section>
      <Message v-model="form.message" label="Message" />
      <Button @click="handleSubmit" className="w-full">Submit</Button>
    </div>
  </main>
</template>
