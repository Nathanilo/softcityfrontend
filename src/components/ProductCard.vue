<script setup>
import { defineProps, ref, computed } from "vue";
import { RouterLink, useRoute, useRouter } from "vue-router";
import { useStore } from "vuex";

const router = useRouter();
const store = useStore();
const route = useRoute();


const props = defineProps({
  product: Object,
});



// Check if user is authenticated
const isAuthenticated = computed(() => store.getters.isAuthenticated);

// Determine the product link based on authentication status
const productLink = computed(() =>
  isAuthenticated.value
    ? "/dashboard/products/" + props.product._id
    : "/products/" + props.product._id
);



const shortDescription = computed(() => {
  let description = props.product.description;
  return description.length > 80
    ? description.slice(0, 80) + "..."
    : description;
});
</script>

<template>
  <div class="p-4">
    <div class="mb-6">
      <h3 class="text-xl font-bold">{{ product.name }}</h3>
    </div>

    <div class="mb-5">
      <img
        :src="product.imageUrl"
        :alt="product.name"
        class="w-full h-48 object-contain rounded-lg"
      />
    </div>

    <p class="text-black mb-2">{{ shortDescription }}</p>

    <div class="border border-gray-100 mb-5"></div>

    <div class="flex flex-col lg:flex-row justify-between mb-4">
      <div class="text-[#e81101] mb-3">
        <p class="text-2xl font-semibold">₦{{ product.price }}</p>
      </div>
      <router-link
        :to="productLink"
        class="h-[36px] bg-gray-700 hover:bg-[#e81101] text-white px-4 py-2 rounded-lg text-center text-sm"
      >
        View More
      </router-link>
    </div>
  </div>
</template>
