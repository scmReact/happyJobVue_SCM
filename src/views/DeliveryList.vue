<template>
    <ContextBox>배송지시서</ContextBox>
    <DeliveryListSearch></DeliveryListSearch>
    <DeliveryListMain></DeliveryListMain>
</template>

<script setup>
import ContextBox from "@/components/common/ContextBox.vue";
import DeliveryListMain from "@/components/page/DeliveryList/DeliveryListMain.vue";
import DeliveryListSearch from "@/components/page/DeliveryList/DeliveryListSearch.vue";
</script>
