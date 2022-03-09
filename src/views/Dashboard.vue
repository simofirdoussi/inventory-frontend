<template>
  <div>
    <h3 class="text-3xl font-medium text-gray-700">Dashboard</h3>

    <div class="mt-4">
      <div class="flex flex-wrap -mx-6">
      
        <div class="w-full px-6 mt-6 sm:w-1/2 xl:w-1/3 xl:mt-0">
         
          <button
          @click="open = true"
          class="px-6 py-3 mt-3 font-medium tracking-wide text-white bg-green-600 rounded-md hover:bg-green-500 focus:outline-none"
        >
          Add Product
        </button>
        </div>
      </div>
    </div>

    <div class="mt-8"></div>

    <div class="flex flex-col mt-8">
      <div class="py-2 -my-2 overflow-x-auto sm:-mx-6 sm:px-6 lg:-mx-8 lg:px-8">
        <div
          class="inline-block min-w-full overflow-hidden align-middle border-b border-gray-200 shadow sm:rounded-lg"
        >
          <table class="min-w-full">
            <thead>
              <tr>
                <th
                  class="px-6 py-3 text-xs font-medium leading-4 tracking-wider text-left text-gray-500 uppercase border-b border-gray-200 bg-gray-50"
                >
                  Name
                </th>
                <th
                  class="px-6 py-3 text-xs font-medium leading-4 tracking-wider text-left text-gray-500 uppercase border-b border-gray-200 bg-gray-50"
                >
                  Price
                </th>
                <th
                  class="px-6 py-3 text-xs font-medium leading-4 tracking-wider text-left text-gray-500 uppercase border-b border-gray-200 bg-gray-50"
                >
                  Status
                </th>
                
                <th class="px-6 py-3 border-b border-gray-200 bg-gray-50"></th>
              </tr>
            </thead>

            <tbody class="bg-white">
              <tr v-for="(p, index) in products" :key="index">
                <td
                  class="px-2 py-4 border-b border-gray-200 whitespace-nowrap"
                >
                  <div class="flex items-center">
                    
                    <div class="ml-4">
                      <div class="text-sm font-medium leading-5 text-gray-900">
                        {{ p.name }}
                      </div>
                      
                    </div>
                  </div>
                </td>

                <td
                  class="px-6 py-4 border-b border-gray-200 whitespace-nowrap"
                >
                  <div class="text-sm leading-5 text-gray-900">
                    ${{p.price}}
                  </div>
                </td>

                <td
                  class="px-6 py-4 border-b border-gray-200 whitespace-nowrap"
                >

                  <span v-if="p.quantity > 0"
                    class="inline-flex px-2 text-xs font-semibold leading-5 text-green-800 bg-green-100 rounded-full"
                    >Available: {{p.quantity}}</span
                  >
                  <span v-else
                    class="inline-flex px-2 text-xs font-semibold leading-5 text-red-800 bg-red-100 rounded-full"
                    >Not Available: {{p.quantity}}</span
                  >
                </td>
                <td
                  class="px-6 py-4 text-sm font-medium leading-5 text-right border-b border-gray-200 whitespace-nowrap"
                >
                  <a v-on:click="getProduct(p.id)" class="text-indigo-600 hover:text-indigo-900"
                    >Modify</a
                  >
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
      <!-- add product modal -->
      <div
          :class="`modal ${
            !open && 'opacity-0 pointer-events-none'
          } z-50 fixed w-full h-full top-0 left-0 flex items-center justify-center`"
        >
          <div
            @click="open = false"
            class="absolute w-full h-full bg-gray-900 opacity-50 modal-overlay"
          ></div>

        <div
          class="z-50 w-11/12 mx-auto overflow-y-auto bg-white rounded shadow-lg modal-container md:max-w-md"
        >
        <div
          class="absolute top-0 right-0 z-50 flex flex-col items-center mt-4 mr-4 text-sm text-white cursor-pointer modal-close"
        >
          <svg
            class="text-white fill-current"
            xmlns="http://www.w3.org/2000/svg"
            width="18"
            height="18"
            viewBox="0 0 18 18"
          >
            <path
              d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"
            />
          </svg>
          <span class="text-sm">(Esc)</span>
        </div>

        <!-- Add margin if you want to see some of the overlay behind the modal-->
        <div class="px-6 py-4 text-left modal-content">
          <!--Title-->
          <div class="flex items-center justify-between pb-3">
            <p class="text-2xl font-bold">Add Product</p>
            <div class="z-50 cursor-pointer modal-close" @click="open = false">
              <svg
                class="text-black fill-current"
                xmlns="http://www.w3.org/2000/svg"
                width="18"
                height="18"
                viewBox="0 0 18 18"
              >
                <path
                  d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"
                />
              </svg>
            </div>
          </div>

          <!--Body-->
          <form @submit.prevent="addProduct">
            <label class="block">
              <input
                type="text"
                v-model="name"
                placeholder="Name"
                class="block w-full mb-2 mt-1 border-gray-200 rounded-md focus:border-indigo-600 focus:ring focus:ring-opacity-40 focus:ring-indigo-500"
              />
            </label>
            <label class="block">
              <input
                type="number"
                v-model="price"
                placeholder="Price"
                class="block w-full mb-2 mt-1 border-gray-200 rounded-md focus:border-indigo-600 focus:ring focus:ring-opacity-40 focus:ring-indigo-500"
              />
            </label>
            <label class="block">
              <input
                type="number"
                v-model="quantity"
                placeholder="Quantity"
                class="block w-full mb-2 mt-1 border-gray-200 rounded-md focus:border-indigo-600 focus:ring focus:ring-opacity-40 focus:ring-indigo-500"
              />
            </label>
     
            <p v-for="error in errors" v-bind:key="error"> {{error}}</p>

            <!--Footer-->
            <div class="flex justify-end pt-2">
              <button
                @click="open = false"
                class="p-3 px-6 py-3 mr-2 text-green-500 bg-transparent rounded-lg hover:bg-gray-100 hover:text-green-400 focus:outline-none"
              >
                Cancel
              </button>
              <button
              type="submit"
                @click="open = false"
                class="px-6 py-3 font-medium tracking-wide text-white bg-green-600 rounded-md hover:bg-green-500 focus:outline-none"
              >
                Save
              </button>
            </div>
        </form>

          
        </div>
      </div>
      </div>
      <!-- edit modal -->
      <div
          :class="`modal ${
            !edit && 'opacity-0 pointer-events-none'
          } z-50 fixed w-full h-full top-0 left-0 flex items-center justify-center`"
        >
        <div
          @click="edit = false"
          class="absolute w-full h-full bg-gray-900 opacity-50 modal-overlay"
        ></div>

        <div
          class="z-50 w-11/12 mx-auto overflow-y-auto bg-white rounded shadow-lg modal-container md:max-w-md"
        >
        <div
          class="absolute top-0 right-0 z-50 flex flex-col items-center mt-4 mr-4 text-sm text-white cursor-pointer modal-close"
        >
          <svg
            class="text-white fill-current"
            xmlns="http://www.w3.org/2000/svg"
            width="18"
            height="18"
            viewBox="0 0 18 18"
          >
            <path
              d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"
            />
          </svg>
          <span class="text-sm">(Esc)</span>
        </div>

        <!-- Add margin if you want to see some of the overlay behind the modal-->
        <div class="px-6 py-4 text-left modal-content">
          <!--Title-->
          <div class="flex items-center justify-between pb-3">
            <p class="text-2xl font-bold">Modify</p>
            <div class="z-50 cursor-pointer modal-close" @click="edit = false">
              <svg
                class="text-black fill-current"
                xmlns="http://www.w3.org/2000/svg"
                width="18"
                height="18"
                viewBox="0 0 18 18"
              >
                <path
                  d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"
                />
              </svg>
            </div>
          </div>

          <!--Body-->
          <form @submit.prevent="confMessage">
            <label class="block">
              <input
                type="text"
                v-model="product.name"
                placeholder="Name"
                class="block w-full mb-2 mt-1 border-gray-200 rounded-md focus:border-indigo-600 focus:ring focus:ring-opacity-40 focus:ring-indigo-500"
              />
            </label>
            <label class="block">
              <input
                type="number"
                v-model="product.price"
                placeholder="Price"
                class="block w-full mb-2 mt-1 border-gray-200 rounded-md focus:border-indigo-600 focus:ring focus:ring-opacity-40 focus:ring-indigo-500"
              />
            </label>
            <!-- {{ product.quantity }} -->
            <div class="flex">

              <a @click="decrementQuantity(product)">
                <span class="material-icons text-2xl text-muted mr-2">remove</span>
              </a>
              <p class="text-2xl"> {{ product.quantity }} </p>  
              <a @click="incrementQuantity(product)">
                <span class="material-icons text-2xl text-muted ml-2">add</span>
              </a>
            </div>
            
            <p v-for="error in errors" v-bind:key="error"> {{error}}</p>

            <!--Footer-->
            <div class="flex justify-end pt-2">
              <button
                @click="edit = false"
                class="p-3 px-6 py-3 mr-2 text-green-500 bg-transparent rounded-lg hover:bg-gray-100 hover:text-green-400 focus:outline-none"
              >
                Cancel
              </button>
              <button
              type="submit"
                class="px-6 py-3 font-medium tracking-wide text-white bg-green-600 rounded-md hover:bg-green-500 focus:outline-none"
              >
                Save
              </button>
            </div>
        </form>

          
        </div>
      </div>
      </div>
      <!-- edit confirm modal -->
      <div
          :class="`modal ${
            !confirm && 'opacity-0 pointer-events-none'
          } z-50 fixed w-full h-full top-0 left-0 flex items-center justify-center`"
        >
        <div
          @click="confirm = false"
          class="absolute w-full h-full bg-gray-900 opacity-50 modal-overlay"
        ></div>

        <div
          class="z-50 w-11/12 mx-auto overflow-y-auto bg-white rounded shadow-lg modal-container md:max-w-md"
        >
        <div
          class="absolute top-0 right-0 z-50 flex flex-col items-center mt-4 mr-4 text-sm text-white cursor-pointer modal-close"
        >
          <svg
            class="text-white fill-current"
            xmlns="http://www.w3.org/2000/svg"
            width="18"
            height="18"
            viewBox="0 0 18 18"
          >
            <path
              d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"
            />
          </svg>
          <span class="text-sm">(Esc)</span>
        </div>

        <!-- Add margin if you want to see some of the overlay behind the modal-->
        <div class="px-6 py-4 text-left modal-content">
          <!--Title-->
          <div class="flex items-center justify-between pb-3">
            <p class="text-xl font-bold">Would you like to continue?</p>
            <div class="z-50 cursor-pointer modal-close" @click="confirm = false">
              <svg
                class="text-black fill-current"
                xmlns="http://www.w3.org/2000/svg"
                width="18"
                height="18"
                viewBox="0 0 18 18"
              >
                <path
                  d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"
                />
              </svg>
            </div>
          </div>

          <!--Body-->
          <form @submit.prevent="confirmModif">
            
            <p v-for="error in errors" v-bind:key="error"> {{error}}</p>

            <!--Footer-->
            <div class="flex justify-end pt-2">
              <button
                @click="confirm = false"
                class="p-3 px-6 py-3 mr-2 text-green-500 bg-transparent rounded-lg hover:bg-gray-100 hover:text-green-400 focus:outline-none"
              >
                Cancel
              </button>
              <button
              type="submit"

                class="px-6 py-3 font-medium tracking-wide text-white bg-green-600 rounded-md hover:bg-green-500 focus:outline-none"
              >
                Yes
              </button>
            </div>
        </form>
        </div>
      </div>
      </div>
    </div>
  </div>
</template>



<script>

export default {
  name: 'Dashboard',
  data() {
    return {
         products: [],
         open: false,
         edit: false,
         confirm: false,
         name:'',
         price:null,
         quantity:null,
         product:[],
         errors:[],    
    }
  },

  computed: {  

  },
  
  mounted() {

  },

  methods: {
    
  },
  
}
</script>


<style>
.dropdown:focus-within .dropdown-menu {
  opacity:1;
  transform: translate(0) scale(1);
  visibility: visible;
}
</style>

