<script setup>
import { useField, useForm } from 'vee-validate'
import { breakpointsTailwind, useBreakpoints } from '@vueuse/core'

const breakpoints = useBreakpoints(breakpointsTailwind)

const smAndLarger = breakpoints.greater('sm')
const mobile = breakpoints.smaller('sm')

const isFormSubmitted = ref(false)

const nameInputEl = ref(null)
const isNameValid = ref(true)

// const validateName = (value) => {
//   if (!value) {
//     isNameValid.value = false
//     // nameInputEl.value.focus()
//     return 'Name is required'
//   }

//   return true
// }

// const validateNumber = (value) => {
//   if (!value)
//     return 'Card number is required'

//   if (value.toString().length !== 16)
//     return 'Card number must be 16 digits'

//   return true
// }

const mySchema = {
  name(value) {
    if (!value) {
      isNameValid.value = false

      return 'Name is required'
    }
    return true
  },
  number(value) {
    if (!value)
      return 'Card number is required'

    if (value.toString().length !== 16)
      return 'Card number must be 16 digits'

    return true
  },
  month(value) {
    if (!value)
      return 'Month is required'

    if (value.toString().length !== 2)
      return 'Month must be 2 digits'

    return true
  },
  year(value) {
    if (!value)
      return 'Year is required'

    if (value.toString().length !== 2)
      return 'Year must be 2 digits'

    return true
  },
  cvc(value) {
    if (!value)
      return 'CVC is required'

    if (value.toString().length !== 3)
      return 'CVC must be 3 digits'

    return true
  },
}
const { handleSubmit } = useForm({ validationSchema: mySchema })
const { value, errorMessage } = useField('name')
const { value: numberValue, errorMessage: numberErrorMessage, handleChange: numberHandleChange } = useField('number')
const { value: monthValue, errorMessage: monthErrorMessage, handleChange: monthHandleChange } = useField('month')
const { value: yearValue, errorMessage: yearErrorMessage } = useField('year')
const { value: cvcValue, errorMessage: cvcErrorMessage } = useField('cvc')

const onSubmit = handleSubmit((values) => {
  console.log('values', values)
})
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
    <form
      v-if="!isFormSubmitted"
      class="sm:ml-[58%] md:ml-[51%] lg:ml-[42%] xl:ml-[34%] md:mr-[3%] lg:mr-[15%] xl:mr-[20%]"
      mt-30
      tracking-widest
      @submit="onSubmit"
    >
      <p text-xs font-bold mb-2>
        CARDHOLDER NAME
      </p>
      <input
        ref="nameInputEl"
        v-model="value"
        class="w-[100%]"
        name="name"
        placeholder="e.g. Jane Appleseed"
        type="text"
        border-1
        border-gray-400
        focus:outline-0
        focus:ring-1
        focus:ring-violet-600
        px-4
        py-2
        rounded-lg
      >
      <span text-xs text-red-600>{{ errorMessage }}</span>

      <p text-xs font-bold mt-5 mb-2>
        CARD NUMBER
      </p>
      <input
        :value="numberValue"
        class="w-[100%]"
        name="number"
        placeholder="e.g. 1234 5678 9123 0000"
        type="number"
        border-1
        border-gray-400
        px-4

        py-2
        rounded-lg
        @blur="numberValue = $event.target.value"
        @change="numberHandleChange"
      >
      <span text-xs text-red-600>{{ numberErrorMessage }}</span>

      <!-- flexbox bottom row -->
      <div flex gap-3 mt-5>
        <div class="basis-1/2">
          <p text-xs font-bold mb-2>
            EXP. DATE (MM/YY)
          </p>
          <div flex gap-2>
            <input
              :value="monthValue"
              class="w-[50%]"
              name="month"
              placeholder="MM"
              type="number"
              border-1
              border-gray-400
              md:px-4
              px-4
              py-2
              rounded-lg
              sm:px-3
              @blur="monthValue = $event.target.value"
              @change="monthHandleChange"
            >
            <input
              :value="yearValue"
              class="w-[50%]"
              name="year"
              placeholder="YY"
              type="number"
              border-1
              border-gray-400
              px-4
              py-2
              rounded-lg
              @blur="yearValue = $event.target.value"
              @change="yearHandleChange"
            >
          </div>
        </div>
        <div class="basis-1/2" self-end>
          <p text-xs font-bold mb-2>
            CVC
          </p>
          <input
            :value="cvcValue"
            name="cvc"
            placeholder="e.g. 123"
            type="number"
            border-1
            border-gray-400
            px-4
            py-2
            rounded-lg
            w-full
            @blur="cvcValue = $event.target.value"
            @change="cvcHandleChange"
          >
        </div>
      </div>
      <span mt-1 block text-xs text-red-600>{{ monthErrorMessage }}</span>
      <span text-xs block mt-1 text-red-600>{{ yearErrorMessage }}</span>
      <span text-xs block mt-1 text-red-600>{{ cvcErrorMessage }}</span>

      <div>
        <button type="submit" mt-7 w-full rounded-lg dark:bg-violet-600 bg-black text-white py-4>
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
