<template>
  <div class="flex items-center justify-center h-screen px-6 bg-gray-200">
    <div class="w-full max-w-sm p-6 bg-white rounded-md shadow-md">
      <div class="flex items-center justify-center">
        <span class="text-2xl font-semibold text-gray-700">Inventory</span>
      </div>

      <form class="mt-4" @submit.prevent="login">
        <label class="block">
          <span class="text-sm text-gray-700">Username</span>
          <input
            type="text"
            class="block w-full mt-1 border-gray-200 rounded-md focus:border-indigo-600 focus:ring focus:ring-opacity-40 focus:ring-indigo-500"
            v-model="username"
          />
        </label>

        <label class="block mt-3">
          <span class="text-sm text-gray-700">Password</span>
          <input
            type="password"
            class="block w-full mt-1 border-gray-200 rounded-md focus:border-indigo-600 focus:ring focus:ring-opacity-40 focus:ring-indigo-500"
            v-model="password"
          />
        </label>

        <div class="flex items-center justify-between mt-4">
            <div v-for="error in errors" v-bind:key="error" >
                    <div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative" role="alert">
                        <span class="block sm:inline">{{error}}</span>
                    </div>
                </div>

        </div>

        <div class="mt-6">
          <button
            type="submit"
            class="w-full px-4 py-2 text-sm text-center text-white bg-green-600 rounded-md focus:outline-none hover:bg-green-500"
          >
            Login
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script lang="ts">

export default {
    name:'Login',
    data() {
        return{
            username:'',
            password:'',
            errors: [],
        }
    },
    methods:{

    },
}
</script>
