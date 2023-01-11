<script setup>
    import axios from 'axios';
    import useProduct from '../composables/products';
    import emitter from 'tiny-emitter/instance';

    const { add } = useProduct();
    const productId = defineProps(['productId']);
    const addToCart = async() => {
        await axios.get('/sanctum/csrf-cookie');
        await axios.get('/api/user')
            .then(async(res) => {
                let cartCount = await add(productId);
                emitter.emit('cartCountUpdated', cartCount);
            })
            .catch(err => console.log(err));
    }
</script>

<template>
    <div class="flex items-center justify-between py-4">
        <button class="bg-indigo-500 text-white p-2"
        @click.prevent="addToCart"
        >Ajouter au panier</button>
    </div>
</template>