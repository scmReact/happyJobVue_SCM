<template>
    <div class="search-box">
        <!-- v-model을 이용하여 양방향 바인딩을 쉽게 할 수 있다. -->
        <!-- 셀렉트 박스 -->
        <label for="searchOption">검색 항목 선택:</label>
        <select v-model="selectedOption" id="searchOption" class="custom-select">
            <option value="keyword">제품명</option>
            <option value="searchManufac">제조사</option>
            <option value="searchItemCode">제품 코드</option>
        </select>

        <!-- 검색 입력 필드 -->
        <label for="searchInput">{{ inputLabel }}</label>
        <input id="searchInput" v-model="searchValue" :placeholder="placeholderText" />
        <!-- v-on:click="" 또는 @click=""으로 이벤트를 설정한다. -->
        <button @click="handlerSearch">검색</button>
    </div>
</template>

<script setup>
import router from "@/router";

const selectedOption = ref("keyword"); // 기본값: 제품명 선택
const searchValue = ref(""); // 사용자 입력 값

const handlerSearch = () => {
    // 1. url 파라미터 생성
    const query = [];
    if (selectedOption.value === "keyword") {
        query.push(`searchItemName=${searchValue.value}`);
    } else if (selectedOption.value === "searchManufac") {
        query.push(`searchManufac=${searchValue.value}`);
    } else if (selectedOption.value === "searchItemCode") {
        query.push(`searchItemCode=${searchValue.value}`);
    }

    const queryString = query.length > 0 ? `?${query.join("&")}` : "";

    // 2. 라우터에 파라미터 push
    router.push(queryString);
};
</script>

<style lang="scss" scoped>
.search-box {
    margin-bottom: 10px;
    display: block;
    float: inline-end;
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
    width: 70px;
    padding-top: 8px;
    padding-bottom: 8px;
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
.custom-select {
    width: 110px; /* 원하는 너비로 조정 */
    padding: 3px; /* 선택 박스의 내부 여백 */
    text-align: center;
    font-size: 17px; /* 텍스트 크기 조정 */
}
</style>
