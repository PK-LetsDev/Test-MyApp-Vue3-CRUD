<template>
    <div class="q-pa-md" style="max-width: 300px">
        <q-form @submit="onSubmit" class="q-gutter-md">
            <q-input v-model="o_id" label="o_id" readonly />
            <q-input v-model="OrderDate" label="OrderDate" />
            <q-input v-model="Region" label="Region" />
            <q-input v-model="City" label="City" />
            <q-input v-model="Category" label="Category" />
            <q-input v-model="Product" label="Product" />
            <q-input v-model="Quantity" label="Quantity" />
            <q-input v-model="UnitPrice" label="Unitprice" />
            <q-input v-model="TotalPrice" label="Totalprice" />
            <q-btn label="Update" type="submit" color="primary" />
        </q-form>
    </div>
</template>
<script setup>
import { ref } from 'vue';
import { useRoute } from 'vue-router';
const route = useRoute();

const o_id = ref(route.params.id)
const OrderDate = ref('')
const Region = ref('')
const City = ref('')
const Category = ref('')
const Product = ref('')
const Quantity = ref('')
const UnitPrice = ref('')
const TotalPrice = ref('')

const fetchData = () => {
    fetch("http://localhost:3000/read/" + o_id.value)
        .then(res => res.json())
        .then((result) => {
            console.log("🚀 ~ file: UpdateView.vue:36 ~ .then ~ result", result)
            OrderDate.value = result.OrderDate
            Region.value = result.Region
            City.value = result.City
            Category.value = result.Category
            Product.value = result.Product
            Quantity.value = result.Quantity
            UnitPrice.value = result.UnitPrice
            TotalPrice.value = result.TotalPrice
        })
}
fetchData()

const onSubmit = () => {
    console.log('o_id', o_id);
    fetch("http://localhost:3000/update/" + o_id.value, {
        method: 'put',
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
        })

        .catch(error => console.log('error', error));
}

</script>