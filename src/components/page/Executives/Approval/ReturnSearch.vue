<template>
    <div class="search-box">
        <div class="radioBtn">
            <input type="radio" value="" name="orderType" v-model="searchKey" @change="returnSearch" checked>전체</input> 
            <input type="radio" value="Y" name="orderType" v-model="searchKey" @change="returnSearch" >승인</input>
            <input type="radio" value="N" name="orderType" v-model="searchKey" @change="returnSearch" >미승인</input>
        </div>
    </div>
</template>

<script setup>
import router from "@/router";
import { useModalStore } from "@/stores/modalState";

const searchKey = ref();
const injectedValue = inject("returnProvidedValue");

const returnSearch = () => {
    injectedValue.value = searchKey.value;
};
</script>

<style lang="scss" scoped>
.search-box {
    float: inline-end;
    margin: 7px 0;
}

select {
    padding: 8px;
    margin-top: 5px;
    margin-bottom: 5px;
    margin-right: 5px;
    border-radius: 4px;
    border: 1px solid #ccc;
}
input {
    padding: 8px;
    margin-top: 5px;
    margin-bottom: 5px;
    margin-right: 5px;
    border-radius: 4px;
    border: 1px solid #ccc;
}
button {
    text-align: center;
    text-decoration: none;
    display: inline-block;
    border: none;
    color: white;
    width: 50px;
    padding-top: 10px;
    padding-bottom: 10px;
    font-size: 12px;
    margin: 4px 2px;
    cursor: pointer;
    border-radius: 12px;
    box-shadow: 0 4px #999;
    background-color: #3bb2ea;

    &:hover {
        background-color: #45a049;
    }

    &:active {
        background-color: #3e8e41;
        box-shadow: 0 2px #666;
        transform: translateY(2px);
    }
}
.radioBtn {
    margin-top: 15px;
    text-align: end;
}
</style>
