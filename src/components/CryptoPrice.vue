<template>
    <div>
        <form @submit.prevent="submitForm">
            <label for="currencySelect">Select Currency:</label>
            <select v-model="selectedCurrency" id="currencySelect">
                <option value="EUR">EUR</option>
                <option value="USD">USD</option>
            </select>
            <label for="priceField">Price:</label>
            <input v-model="formattedPrice" id="priceField" type="text" readonly />
            <button type="submit">Submit</button>
        </form>

        <!-- Display the collected EUR data -->
        <h2>EUR Data</h2>
        <ul>
            <li v-for="(data, index) in euroDataList" :key="index">
                Currency: {{ data.currency }}, Price: {{ data.formattedPrice }}
            </li>
        </ul>

        <!-- Display the collected USD data -->
        <h2>USD Data</h2>
        <ul>
            <li v-for="(data, index) in usdDataList" :key="index">
                Currency: {{ data.currency }}, Price: {{ data.formattedPrice }}
            </li>
        </ul>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
    data() {
        return {
            selectedCurrency: 'EUR',
            price: 0,
            euroDataList: [],
            usdDataList: [],
        };
    },

    computed: {
        formattedPrice() {
            if (this.selectedCurrency === 'EUR') {
                return `€${this.price}`;
            } else if (this.selectedCurrency === 'USD') {
                return `$${this.price}`;
            } else {
                return this.price;
            }
        },
    },

    created() {
        this.fetchData();
    },

    methods: {
        async fetchData() {
            // Construct the API URL based on the selected currency
            const apiUrl = `https://api.coinbase.com/v2/prices/spot?currency=${this.selectedCurrency}`;

            try {
                const response = await axios.get(apiUrl);
                this.price = response.data.data.amount; // Assumes the price data is under the "amount" key
            } catch (error) {
                console.error('Error fetching data:', error);
            }
        },

        submitForm() {
            const collectedData = {
                currency: this.selectedCurrency,
                formattedPrice: this.formattedPrice,
            };

            if (this.selectedCurrency === 'EUR') {
                this.euroDataList.push(collectedData);
            } else if (this.selectedCurrency === 'USD') {
                this.usdDataList.push(collectedData);
            }
            this.$emit('data-updated', this.euroDataList, this.usdDataList);
        },
    },
};
</script>
  