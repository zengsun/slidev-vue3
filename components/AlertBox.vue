<template>
  <div class="my-2 bg-teal-500 text-white">
    <div class="flex items-center p-2">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        class="w-8 h-8 mx-2 stroke-current"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
        ></path>
      </svg>
      <label class="leading-normal"><slot /></label>
    </div>
  </div>
</template>
