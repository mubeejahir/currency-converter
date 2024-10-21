<template>
  <div>
    <h1>Currency Converter</h1>
    <br />
    <select v-model="fromCurrency">
      <option value="USD">USD (US Dollar)</option>
      <option value="EUR">EUR (Euro)</option>
      <option value="JPY">JPY (Japanese Yen)</option>
      <option value="SGD">SGD (Singapore Dollar)</option>
      <option value="AUD">AUD (Austrlian Dollar)</option>
      <option value="GBP">GBP (British Pound)</option>
    </select>

    <select v-model="toCurrency">
      <option value="USD">USD (US Dollar)</option>
      <option value="EUR">EUR (Euro)</option>
      <option value="JPY">JPY (Japanese Yen)</option>
      <option value="SGD">SGD (Singapore Dollar)</option>
      <option value="AUD">AUD (Austrlian Dollar)</option>
      <option value="GBP">GBP (British Pound)</option>
    </select>

    <br />
    <input type="number" v-model="amount" placeholder="Enter amount" />

    <br /><br />

    <button @click="convertCurrency">Convert</button>
    <p>{{ currency }}</p>

    <div v-if="conversionRate && amount">
      <h1>
        {{ amount }} {{ fromCurrency }} equals {{ amount * conversionRate }}
        {{ toCurrency }}
      </h1>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { useFetch } from "#imports";

export default {
  setup() {
    const apiKey = "QVHOZN7287COAQI9";
    const fromCurrency = ref("USD");
    const toCurrency = ref("EUR");
    const conversionRate = ref(null);
    const amount = ref("");

    const { data: currency, execute } = useFetch(
      () => {
        return `https://www.alphavantage.co/query?function=CURRENCY_EXCHANGE_RATE&from_currency=${fromCurrency.value}&to_currency=${toCurrency.value}&apikey=${apiKey}`;
      },
      {
        immediate: false,
        transform: (data) => {
          const convert =
            data["Realtime Currency Exchange Rate"]["5. Exchange Rate"];
          conversionRate.value = convert;
          console.log(conversionRate);
        },
      }
    );
    console.log(currency.value);

    const convertCurrency = () => {
      console.log("clicked");
      execute();
    };

    return {
      currency,
      fromCurrency,
      toCurrency,
      conversionRate,
      amount,
      convertCurrency,
    };
  },
};
</script>
