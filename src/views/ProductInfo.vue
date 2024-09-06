<template>
    <ContextBox>제품 정보 관리</ContextBox>
    <ProductInfoSearch></ProductInfoSearch>
    <ProductInfoMain></ProductInfoMain>
</template>

<script setup>
import ContextBox from "@/components/common/ContextBox.vue";
import ProductInfoMain from "@/components/ProductInfo/ProductInfoMain.vue";
import ProductInfoSearch from "@/components/ProductInfo/ProductInfoSearch.vue";

// 제공할 데이터
const providedValue = ref();

// 데이터 제공
provide("providedValue", providedValue);
</script>
