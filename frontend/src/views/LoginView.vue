<template>
  <div class="pt-16">
    <h1 class="text-3xl font-semibold mb-4">Enter your phone number</h1>
    <form v-if="!waitingForVerification" action="#" @submit.prevent="handleLogin">
      <div
        class="overflow-hidden shadow sm:rounded-md max-w-sm mx-auto text-left"
      >
        <div class="bg-white px-4 py-5 sm:p-6">
          <div>
            <input
              type="text"
              v-maska
              data-maska="+##### ####-####"
              v-model="data.phone"
              name="phone"
              id="phone"
              placeholder="+62812 3456 7890"
              class="mt-1 block w-full px-3 py-2 rounded-md border border-gray-300 shadow-sm"
            />
          </div>
        </div>
        <div class="bg-gray-50 px-4 py-3 text-right sm:px-6">
          <button
            type="submit"
            @submit.prevent="handleLogin"
            class="inline-flex justify-center rounded-md border border-transparent bg-black py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-gray-600 focus:outline-none"
          >
            Continue
          </button>
        </div>
      </div>
    </form>

    <form v-else action="#" @submit.prevent="handleVerif">
      <div
        class="overflow-hidden shadow sm:rounded-md max-w-sm mx-auto text-left"
      >
        <div class="bg-white px-4 py-5 sm:p-6">
          <div>
            <input
              type="number"
              v-maska
              data-maska="######"
              v-model="data.login_code"
              name="login_code"
              id="login_code"
              placeholder="######"
              class="mt-1 block w-full px-3 py-2 rounded-md border border-gray-300 shadow-sm"
            />
          </div>
        </div>
        <div class="bg-gray-50 px-4 py-3 text-right sm:px-6">
          <button
            type="submit"
            @submit.prevent="handleVerif"
            class="inline-flex justify-center rounded-md border border-transparent bg-black py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-gray-600 focus:outline-none"
          >
            Continue
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script setup>
import { vMaska } from "maska";
import { ref, reactive } from "vue";
import axios from 'axios'
import { useRouter } from "vue-router";

const router = useRouter()

onMounted(() => {
  if (localStorage.getItem('letsnebengToken')) {
    router.push({
      name:"landing"
    })
  }
})

const data = reactive({
  phone: null,
  login_code: null
});

const waitingForVerification = ref(false)

const handleLogin = () => {
  const trimPhone = data.phone.replaceAll(' ', '').replace('-', '')
  axios.post('http://127.0.0.1:8000/api/login', {
    phone: trimPhone
  })
  .then((res) => {
    waitingForVerification.value = true
    console.log(res.data);
  })
  .catch((err)=>{
    console.error(err);
    alert(err.response.data.message)
  })
};

const handleVerif = () => {
  const trimPhone = data.phone.replaceAll(' ', '').replace('-', '')
  axios.post('http://127.0.0.1:8000/api/login/verify', {
    phone: trimPhone,
    login_code: data.login_code
  })
  .then((res) => {
    console.log(res.data);
    localStorage.setItem("letsnebengToken", res.data)
    router.push({
      name:"landing"
    })
  })
  .catch((err)=>{
    console.error(err);
    alert(err.response.data.message)
  })
}
</script>

<style></style>
