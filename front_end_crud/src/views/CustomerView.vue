<template>
<Header />
<main>
    <div class="row m-0">
        <div class="col-md-6 p-0">
            <div class="card filter-card m-2 text-center">
                <h6 id="total_customers" class="p-4">Total Customers</h6>
                <h4><strong class="m-4">{{ totalCustomers }}</strong></h4>
            </div>
        </div>
        <div class="col-md-6 p-0">
            <div class="card filter-card m-2 text-center">
                <h6 class="p-4">Active Customer</h6>
                <h4><strong class="m-4">{{ activeCustomers }}</strong></h4>
            </div>
        </div>
        <div class="col-md-6 p-0">
            <div class="card filter-card m-2 text-center">
                <h6 class="p-4">Inactive Customers</h6>
                <h4><strong class="m-4">{{ inactiveCustomers }}</strong></h4>
            </div>
        </div>

    </div>
    <AddCustomer />
    <CustomerList />
</main>
<Footer />
</template>

  
<script>
import Header from '../components/base/Header.vue';
import Footer from '../components/base/Footer.vue';
import CustomerList from '../components/Customer/CustomerList.vue'
import AddCustomer from '../components/Customer/AddCustomer.vue'
import axios from 'axios';

export default {
    components: {
        Header,
        CustomerList,
        AddCustomer,
        Footer,
    },
    data() {
        return {
            totalCustomers: 0,
            activeCustomers: 0,
            inactiveCustomers: 0, // Add this line
            pollInterval: null,
        };
    },
    mounted() {
        this.fetchCustomerStatistics();
        // Set up a poll interval to fetch data every 5 seconds
        this.pollInterval = setInterval(this.fetchCustomerStatistics, 5000);
    },
    beforeDestroy() {
        // Clear the poll interval when the component is destroyed
        clearInterval(this.pollInterval);
    },
    methods: {
        async fetchCustomerStatistics() {
            try {
                // Replace 'your-api-endpoint' with the actual API endpoint
                // const response = await axios.get('http://127.0.0.1:8000/customer-statistics/');
                const response = await axios.get('https://khawarkhan.pythonanywhere.com/customer-statistics/');
                const data = response.data;

                // Assuming your API response structure has a property 'totalCustomers' and 'activeCustomers'
                this.totalCustomers = data.totalCustomers;
                this.activeCustomers = data.activeCustomers;
                this.inactiveCustomers = data.inactiveCustomers; // Add this line
            } catch (error) {
                console.error('Error fetching customer statistics:', error);
            }
        },
    },
};
</script>

  
<style scoped>
  </style>
