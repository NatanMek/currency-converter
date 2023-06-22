<template>
  <div class="w-full max-w-2xl m-auto mt-20 mr-25">
    <form class="rounded-lg shadow-2xl bg-slate-100 px-8 py-8" @submit.prevent="submit()">
      <div class="-mx-3 mb-6 flex">
        <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
          <label
            class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
            for="grid-first-name"
          >
            Amount
          </label>
          <input
            class="font-bold appearance-none block w-full bg-gray-200 text-gray-700 border rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
            id="amount"
            type="number"
            placeholder="0.00"
            v-model.trim="amount"
          />
        </div>

        <div class="w-full md:w-1/3 px-3 mb-6 md:mb-0">
          <label
            class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
            for="grid-state"
          >
            From
          </label>
          <div class="relative">
            <select
              class="block appearance-none w-full bg-gray-200 border border-gray-200 text-gray-700 py-3 px-4 pr-6 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
              id="grid-state"
              v-model="fromCurrency"
            >
              <option>$USD</option>
              <option>€EUR</option>
              <option>£GBP</option>
              <option>$CAD</option>
              <option>$AUD</option>
              <option>¥JPY</option>
              <option>*NOK</option>
              <option>*SEK</option>
            </select>
            <div
              class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700"
            >
              <svg
                class="fill-current h-4 w-4"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
              >
                <path
                  d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
                />
              </svg>
            </div>
          </div>
        </div>
        <div>
          <button
            class="bg-indigo-600 hover:bg-indigo-400 text-white font-bold rounded-full w-10 h-10 mt-6 ml-2"
            type="button"
            @click="swapCurrency"
          >
            <svg xmlns="http://www.w3.org/2000/svg" height="1.25em" viewBox="0 0 448 512">
              <!--! Font Awesome Free 6.4.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
              <path
                d="M438.6 150.6c12.5-12.5 12.5-32.8 0-45.3l-96-96c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L338.7 96 32 96C14.3 96 0 110.3 0 128s14.3 32 32 32l306.7 0-41.4 41.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0l96-96zm-333.3 352c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L109.3 416 416 416c17.7 0 32-14.3 32-32s-14.3-32-32-32l-306.7 0 41.4-41.4c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0l-96 96c-12.5 12.5-12.5 32.8 0 45.3l96 96z"
              />
            </svg>
          </button>
        </div>

        <div class="w-full md:w-1/3 px-3 mb-6 md:mb-0">
          <label
            class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
            for="grid-state"
          >
            To
          </label>
          <div class="relative">
            <select
              class="block appearance-none w-full bg-gray-200 border border-gray-200 text-gray-700 py-3 px-4 pr-6 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
              id="grid-state"
              v-model="toCurrency"
            >
              <option>$USD</option>
              <option>€EUR</option>
              <option>£GBP</option>
              <option>$CAD</option>
              <option>¥JPY</option>
              <option>*NOK</option>
              <option>*SEK</option>
            </select>
            <div
              class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700"
            >
              <svg
                class="fill-current h-4 w-4"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
              >
                <path
                  d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
                />
              </svg>
            </div>
          </div>
        </div>
        <div class="flex items-center justify-between">
          <button
            type="submit"
            class="bg-indigo-600 hover:bg-indigo-400 text-white font-bold rounded-full w-20 h-10 mt-2 ml-3"
          >
            Get Rate
          </button>
        </div>
      </div>
      <transition name="rate">
        <div v-if="results != null">
          <p class="text-black text-2xl font-bold">
            <strong>{{ toCurrency + ' ' + results.converted }}</strong>
          </p>
          <br />
          <p class="text-indigo-400 text-xl font-bold">
            <strong
              >The Current Rate is: 1 {{ fromCurrency.slice(1) }} =
              {{ results.rate + ' ' + toCurrency.slice(1) }}</strong
            >
          </p>
          <br />
          <button
            type="button"
            class="bg-indigo-600 hover:bg-indigo-400 text-white font-bold rounded-full w-20 h-10 mt-2"
            @click="cleanData"
          >
            Refresh
          </button>
        </div>
        <div v-else-if="error != null">
          <h2 class="font-bold text-red-500 text-center">{{ error }}</h2>
        </div>
      </transition>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      error: null,
      amount: null,
      fromCurrency: '€EUR',
      toCurrency: '$USD',
      results: null
    }
  },
  methods: {
    async submit() {
      const response = await fetch(
        `https://anyapi.io/api/v1/exchange/convert?base=${this.fromCurrency.slice(
          1
        )}&to=${this.toCurrency.slice(1)}&amount=${
          this.amount
        }&apiKey=t0mnij42868ul8afmrp96sjqgbsohlfphponetc8borlqp3qv62ug`,
        { method: 'GET' }
      )
      const resData = await response.json()

      if (response.status !== 200) {
        this.error = 'An Error occured! Please try again!'
        setTimeout(() => {
          this.error = null
        }, 5000)
      } else {
        this.results = resData
        this.amount = null
      }
    },

    swapCurrency() {
      const tempCurr = this.fromCurrency
      this.fromCurrency = this.toCurrency
      this.toCurrency = tempCurr
    },

    cleanData() {
      this.results = null
    }
  }
}
</script>

<style scoped>
button > svg {
  width: 40px;
  fill: #fff;
}
.rate-enter-from {
  opacity: 0;
  transform: translateX(-30px);
}

.rate-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.rate-enter-active {
  transition: all 0.4s ease-out;
}

.rate-leave-active {
  transition: all 0.4s ease-in;
}

.rate-enter-to,
.rate-leave-from {
  opacity: 1;
  transform: translateX(0);
}
</style>
