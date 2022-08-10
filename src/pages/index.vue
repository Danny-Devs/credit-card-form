<script setup>
import { ErrorMessage, Field, Form } from 'vee-validate'
import { breakpointsTailwind, useBreakpoints } from '@vueuse/core'

const breakpoints = useBreakpoints(breakpointsTailwind)

const smAndLarger = breakpoints.greater('sm')
const mobile = breakpoints.smaller('sm')

const isFormSubmitted = ref(false)

const cardholderName = ref(null)
const cardNumber = ref(null)
const month = ref(null)
const year = ref(null)
const cvc = ref(null)
</script>

<template>
  <main :class="mobile ? 'mobileBackground' : 'desktopBackground'" h-screen px-6 pt-6>
    <!-- light/dark toggle -->
    <div flex justify-end mb-5>
      <div @click="toggleDark()">
        <div v-if="isDark" i-carbon-sun />
        <div v-else text-white i-carbon-moon />
      </div>
    </div>

    <!-- card images for mobile -->
    <div v-if="mobile">
      <div pl-12>
        <img src="/bg-card-back.png">
      </div>
      <div pr-14 absolute class="top-35 left-6">
        <img src="/bg-card-front.png">
      </div>
      <div absolute top-40 left-11>
        <img src="/card-logo.svg" class="w-[62px]">
      </div>
      <div absolute text-white text-xl tracking-widest left-11>
        9591 6489 6389 1011
      </div>
      <div class="w-[75%]" justify-between flex text-white text-xs absolute top-68 tracking-widest>
        <div ml-5>
          FELICIA LEIRE
        </div>
        <div>
          09/26
        </div>
      </div>
    </div>
    <!-- end card images for mobile -->

    <!-- card images for desktop -->
    <div v-if="!mobile">
      <div sm:ml-1 md:ml-9 lg:ml-20 absolute top-84 class="w-[310px]">
        <img src="/bg-card-back.png">
      </div>
      <div md:ml-9 lg:ml-20 absolute class="top-35 left-4 w-[310px]">
        <img src="/bg-card-front.png">
      </div>
      <div md:ml-9 lg:ml-20 absolute top-40 left-11>
        <img src="/card-logo.svg" class="w-[62px]">
      </div>
      <div md:ml-9 lg:ml-20 absolute text-white text-xl top-56 tracking-widest left-11>
        9591 6489 6389 1011
      </div>
      <div md:ml-9 lg:ml-20 class="w-[275px]" justify-between flex text-white text-xs absolute top-68 tracking-widest>
        <div ml-5>
          FELICIA LEIRE
        </div>
        <div>
          09/26
        </div>
      </div>
    </div>
    <!-- end card images for desktop -->

    <!-- form -->
    <Form v-if="!isFormSubmitted" tracking-widest mt-30 class="sm:ml-[58%] md:ml-[51%] lg:ml-[42%] xl:ml-[34%] md:mr-[3%] lg:mr-[15%] xl:mr-[20%]" @submit="handleSubmit">
      <p text-xs font-bold mb-2>
        CARDHOLDER NAME
      </p>
      <Field v-model="cardholderName" name="cardholder-name" mb-5 border-1 border-gray-400 px-4 py-2 rounded-lg type="text" class="w-[100%]" placeholder="e.g. Jane Appleseed" />

      <p text-xs font-bold mb-2>
        CARD NUMBER
      </p>
      <Field v-model="cardNumber" name="card-number" type="number" class="w-[100%]" mb-5 border-1 border-gray-400 px-4 py-2 rounded-lg placeholder="e.g. 1234 5678 9123 0000" />
      <!-- flexbox bottom row -->
      <div flex gap-3 mb-7>
        <div class="basis-1/2">
          <p text-xs font-bold mb-2>
            EXP. DATE (MM/YY)
          </p>
          <div flex gap-2>
            <Field v-model="month" name="month" type="number" class="w-[50%]" placeholder="MM" border-1 border-gray-400 px-4 sm:px-3 md:px-4 py-2 rounded-lg />
            <Field v-model="year" name="year" type="number" class="w-[50%]" placeholder="YY" border-1 border-gray-400 px-4 py-2 rounded-lg />
          </div>
        </div>
        <div class="basis-1/2" self-end>
          <p text-xs font-bold mb-2>
            CVC
          </p>
          <Field v-model="cvc" name="cvc" type="number" w-full placeholder="e.g. 123" border-1 border-gray-400 px-4 py-2 rounded-lg />
        </div>
      </div>

      <div>
        <button type="submit" w-full rounded-lg dark:bg-violet-600 bg-black text-white py-4>
          Confirm
        </button>
      </div>
    </Form>

    <!-- successful submission screen -->
    <div v-if="isFormSubmitted" mt-30 class="sm:ml-[58%] md:ml-[51%] lg:ml-[42%] xl:ml-[34%] md:mr-[3%] lg:mr-[15%] xl:mr-[20%]">
      <div mb-8>
        <img mx-auto src="/icon-complete.svg">
      </div>

      <div text-center>
        <h2 mb-4 text-3xl font-bold tracking-widest>
          THANK YOU!
        </h2>
        <p mb-8 text-gray-500 text-sm>
          We've added your card details
        </p>
      </div>
      <div>
        <button w-full rounded-lg dark:bg-violet-600 bg-black text-white py-4 @click="isFormSubmitted = !isFormSubmitted">
          Continue
        </button>
      </div>
    </div>
  </main>
</template>

<style scoped>
.mobileBackground {
  background-image: url('/bg-main-mobile.png');
  background-repeat: no-repeat;
  background-size: 100%;
}

.desktopBackground {
  background-image: url('/bg-main-desktop.png');
  background-repeat: no-repeat;
  background-size: contain;
}

/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}
</style>

<route lang="yaml">
meta:
  layout: home
</route>
