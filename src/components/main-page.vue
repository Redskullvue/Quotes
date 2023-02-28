<template>
  <div
    class="h-screen w-screen flex items-center justify-center transition-colors duration-500"
    :class="mainColor.bg"
  >
    <div class="w-screen bg-white px-8 py-4 text-center md:w-1/3">
      <!-- Quote Container -->
      <div class="m-4 flex flex-col">
        <p
          v-if="quote != {}"
          class="font-bold text-xl text-center transition-colors duration-500"
          :class="mainColor.text"
        >
          {{ quote.text }}
        </p>
        <p class="font-bold text-xl text-center" v-if="isBad">
          Check Your Internet Connection :)))
        </p>
        <div
          class="font-semibold text-md text-end transition-colors duration-500"
          :class="mainColor.text"
        >
          <!-- Quote Person -->
          <div>-{{ quote.author }}</div>
        </div>
        <div class="w-full flex items-center justify-between mt-6">
          <!-- Share Buttons -->
          <div>
            <button
              class="mx-1 p-2 rounded-md transition-colors duration-500"
              :class="mainColor.bg"
              @click="share"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="white"
                class="w-5 h-5"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M7.217 10.907a2.25 2.25 0 100 2.186m0-2.186c.18.324.283.696.283 1.093s-.103.77-.283 1.093m0-2.186l9.566-5.314m-9.566 7.5l9.566 5.314m0 0a2.25 2.25 0 103.935 2.186 2.25 2.25 0 00-3.935-2.186zm0-12.814a2.25 2.25 0 103.933-2.185 2.25 2.25 0 00-3.933 2.185z"
                />
              </svg>
            </button>
          </div>
          <!-- Next BUtton -->
          <button
            @click="changeQuote"
            class="text-white p-2 rounded-md transition-colors duration-500"
            :class="mainColor.bg"
          >
            Next Quote
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "mainPage",

  data() {
    return {
      quote: {},
      allQuotes: [],
      isBad: false,
      colors: [
        { bg: "bg-blue-400", text: "text-blue-400" },
        { bg: "bg-green-400", text: "text-green-400" },
        { bg: "bg-red-400", text: "text-red-400" },
        { bg: "bg-orange-400", text: "text-orange-400" },
        { bg: "bg-black", text: "text-black" },
      ],
      mainColor: "",
    };
  },
  created() {
    axios
      .get("https://type.fit/api/quotes")
      .then((res) => {
        this.quote = res.data[this.randomNumber(1643)];
        this.allQuotes = res.data;
        this.mainColor = this.colors[this.randomNumber(5)];
      })
      .catch((err) => {
        console.log(err);
        this.isBad = true;
      });
  },
  methods: {
    randomNumber(max) {
      return Math.floor(Math.random() * max);
    },
    changeQuote() {
      let newQuote = this.allQuotes[this.randomNumber(1643)];
      this.quote = newQuote;
      this.mainColor = this.colors[this.randomNumber(5)];
    },
    share() {
      navigator.share(this.quote);
    },
  },
};
</script>

<style scoped></style>
