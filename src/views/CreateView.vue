<template>
    <div class="q-pa-md" style="max-width: 300px">
        <q-form @submit="onSubmit" class="q-gutter-md">
            <q-input v-model="OrderDate" label="Orderdate" />
            <q-input v-model="Region" label="Region" />
            <q-input v-model="City" label="City" />
            <q-input v-model="Category" label="Category" />
            <q-input v-model="Product" label="Product" />
            <q-input v-model="Quantity" label="Quantity" />
            <q-input v-model="UnitPrice" label="Unitprice" />
            <q-input v-model="TotalPrice" label="Totalprice" />
            <q-btn label="Submit" type="submit" color="primary" />
        </q-form>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import router from '../router';
const OrderDate = ref('')
const Region = ref('')
const City = ref('')
const Category = ref('')
const Product = ref('')
const Quantity = ref('')
const UnitPrice = ref('')
const TotalPrice = ref('')

const onSubmit = () => {
    fetch("http://localhost:3000/create", {
        method: 'post',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            "OrderDate": OrderDate.value,
            "Region": Region.value,
            "City": City.value,
            "Category": Category.value,
            "Product": Product.value,
            "Quantity": Quantity.value,
            "UnitPrice": UnitPrice.value,
            "TotalPrice": TotalPrice.value
        }),
    })
        .then(response => response.json())
        .then(result => {
            alert(result.massage)
            if (result.status === 'ok') {
                router.push('/')
            }
        })

        .catch(error => console.log('error', error));
}

</script>
    