<template>
  <div class="flex flex-col min-h-screen">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8 mt-8 flex-grow">
      <main class="flex justify-between">

<div class="hidden md:block md:w-1/4">
  <div class="bg-white rounded-md space-y-2 my-8">
    <div class="p-10">
    <h2 class="pb-2 font-semibold text-gray-800">Search commands</h2>
<form class="w-full flex md:ml-0">
            <label for="search_field" class="sr-only">Search commands</label>

            <div
              class="relative text-gray-400 focus-within:text-gray-600 min-w-full"
            >
              <div
                class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
              >
                <svg
                  class="h-5 w-5"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                  aria-hidden="true"
                >
                  <path
                    fill-rule="evenodd"
                    d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414   1.414l-4.816-4.816A6 6 0 012 8z"
                    clip-rule="evenodd"
                  />
                </svg>
              </div>

              <input
                id="text"
                ref="search"
                type="search"
                class="py-3 px-4 bg-gray-100 placeholder-gray-400 text-gray-900 rounded-lg shadow-md appearance-none w-full block pl-12 focus:outline-none"
                placeholder="Search"
                tabindex="0"
                spellcheck="false"
                autocomplete="off"
                @input="handleFilter"
                @keypress.enter.prevent
              />
            </div>
          </form>
          </div>
  </div>

<div class="bg-white rounded-md shadow">
  <div class="p-10">
  <h2 class="pb-2 font-semibold text-gray-800">List commands</h2>
  <command-link
  v-for="command in commands"
            :key="command.name"
            :command="command"
  />
  </div>
</div>

</div>

<div class="md:w-8/12 w-full">
        <div class="mt-2 md:hidden">
          <form class="w-full flex md:ml-0">
            <label for="search_field" class="sr-only">Search</label>

            <div
              class="relative text-gray-400 focus-within:text-gray-600 min-w-full"
            >
              <div
                class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
              >
                <svg
                  class="h-5 w-5"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                  aria-hidden="true"
                >
                  <path
                    fill-rule="evenodd"
                    d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414   1.414l-4.816-4.816A6 6 0 012 8z"
                    clip-rule="evenodd"
                  />
                </svg>
              </div>

              <input
                id="text"
                ref="search"
                type="search"
                class="py-3 px-4 bg-white placeholder-gray-400 text-gray-900 rounded-lg shadow-md appearance-none w-full block pl-12 focus:outline-none"
                placeholder="Search"
                tabindex="0"
                spellcheck="false"
                autocomplete="off"
                @input="handleFilter"
                @keypress.enter.prevent
              />
            </div>
          </form>

        </div>

        <div class="space-y-8 my-8">
          <div v-if="commands.length == 0">
            <div
              class="rounded-xl shadow-lg overflow-hidden bg-white p-10 text-center"
            >
              <h1 class="text-xl font-bold text-indigo-900">
                No Commands Found
              </h1>
              <p>
                Nothing found for <code class="font-mono">{{ filter }}</code>
              </p>
            </div>
          </div>

          <command
            v-for="command in commands"
            :key="command.name"
            :command="command"
          />
        </div>

</div>

      </main>
    </div>

  </div>
</template>

<script>
import data from '../assets/8.x.json'
const Mousetrap = require('mousetrap')

export default {
  data() {
    return {
      data: [],
      filter: '',
    }
  },
  computed: {
    commands() {
      if (!this.filter.length) {
        return this.data
      }

      const keyword = this.filter.toLowerCase()

      return this.data.filter((command) => {
        if (
          command.name.toLowerCase().includes(keyword) ||
          command.synopsis.toLowerCase().includes(keyword) ||
          command.description.toLowerCase().includes(keyword)
        ) {
          return command
        }
      })
    },
  },
  created() {
    this.data = data
  },
  mounted() {
    Mousetrap.bind(['command+k', 'ctrl+k'], (event) => {
      this.$refs.search.focus()

      return false
    })
  },
  methods: {
    handleFilter(event) {
      this.filter = event.target.value
    },
  },
}
</script>
