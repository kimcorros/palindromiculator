<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="4">
      <v-card class="text-center d-flex flex-column align-center py-12 px-10">
        <v-card-title class="display-1 font-weight-bold mb-6">
          Palindromic Number Calculator
        </v-card-title>
        <v-card-text>
          <p class="mb-10">
            Find the largest palindromic number that can be formed by the
            product of two x-digit numbers, where x is a positive integer
            greater than 1.
          </p>
          <v-text-field
            v-model="number"
            dense
            outlined
            filled
            class="my-5"
            label="Enter a number"
            hint="Enter a positive integer greater than 1"
          />
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn
            color="primary"
            x-large
            class="px-5"
            :loading="isCalculating"
            @click="calculatePalindromeProduct(number)"
          >
            Calculate
          </v-btn>
        </v-card-actions>
        <v-card-text
          v-if="palindromeProduct"
          class="d-flex flex-column align-center"
        >
          <p class="headline">
            {{ palindromeMultipliers[0] }} x {{ palindromeMultipliers[1] }}
          </p>
          <h1 class="display-2 font-weight-bold">{{ palindromeProduct }}</h1>
          <p class="mt-4 subtitle-2">Execution time: {{ executionTime }}ms</p>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      number: null,
      palindromeProduct: null,
      palindromeMultipliers: [],
      executionTime: null,
      isCalculating: false,
    }
  },
  methods: {
    // Checks if the product of 2 input numbers is palindrome
    checkPalindromeNumber(num1, num2) {
      const prod = num1 * num2

      if (prod.toString() === prod.toString().split('').reverse().join('')) {
        return true
      }

      return false
    },

    calculatePalindromeProduct(num) {
      this.isCalculating = true
      const start = performance.now() // start of calculation
      const max = Math.pow(10, num) - 1 // Starting value of the loops
      // Values for minimum is set higher because the pattern shows that the
      // largest palindrome product does not go lower to 90% of the dataset
      const min = max - Math.pow(10, num - 1)

      for (let i = max; min < i; i--) {
        // boolean flag to break out of the loop once value is found
        let found = false
        for (let j = i; min < j; j--) {
          if (this.checkPalindromeNumber(i, j)) {
            this.palindromeProduct = i * j
            this.palindromeMultipliers[0] = i
            this.palindromeMultipliers[1] = j
            found = true
            break
          }
        }
        if (found) break
      }

      const end = performance.now() // end of execution
      this.executionTime = end - start
      this.isCalculating = false
    },
  },
}
</script>
