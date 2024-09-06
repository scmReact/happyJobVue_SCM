<template>
    <div class="divNoticeList">
        현재 페이지: {{ cPage }} 총 개수: {{ productList?.listCount }}
        <table>
            <colgroup>
                <col width="25%" />
                <col width="25%" />
                <col width="25%" />
                <col width="25%" />
            </colgroup>

            <thead>
                <tr>
                    <th scope="col">제품 코드</th>
                    <th scope="col">제품명</th>
                    <th scope="col">제조사</th>
                    <th scope="col">판매가</th>
                </tr>
            </thead>
            <tbody>
                <template v-if="productList">
                    <template v-if="productList.listCount > 0">
                        <!-- v-bind:key="" 또는 :key=""를 사용하여 속성의 속성값을 바꿀 수 있다. (v-bind:속성="값" 또는 :속성="값") -->
                        <tr
                            v-for="item in productList.productList"
                            :key="item.item_code"
                            @click="handlerUpdateModal(item)"
                        >
                            <td>{{ item.item_code }}</td>
                            <td>{{ item.item_name }}</td>
                            <td>{{ item.manufac }}</td>
                            <td>{{ item.provide_value }}</td>
                        </tr>
                    </template>
                    <template v-else>
                        <tr>
                            <td colspan="4">일치하는 검색 결과가 없습니다</td>
                        </tr>
                    </template>
                </template>
            </tbody>
            <ProductInfoModal v-if="modalState.modalState" :itemCodeProp="itemCodeProp"></ProductInfoModal>
            <button @click="handlerInsertModal()">제품등록</button>
        </table>
        <Pagination
            :totalItems="productList?.listCount"
            :itemsPerPage="5"
            :maxPagesShown="5"
            :onClick="searchList"
            v-model="cPage"
        />
    </div>
</template>

<script setup>
import Pagination from "@/components/common/Pagination.vue";
import axios from "axios";
import { useModalStore } from "@/stores/modalState";
import ProductInfoModal from "./ProductInfoModal.vue";

const injectedValue = inject("providedValue");
const route = useRoute();
const productList = ref();
const cPage = ref(1);
const modalState = useModalStore();
const item_code = ref(0);
const itemCodeProp = ref();

const searchList = () => {
    let param = new URLSearchParams({
        cpage: cPage.value,
        pageSize: 5,
        searchTitle: route.query.searchTitle || "",
        searchStDate: route.query.searchStDate || "",
        searchEdDate: route.query.searchEdDate || ""
    });

    axios.post("/api/management/getProductListJson.do", param).then((res) => {
        productList.value = res.data;
    });
};

// route 값이 변경되면 searchList가 실행된다.
watch(route, searchList);

onMounted(() => {
    searchList();
});

const handlerInsertModal = () => {
    itemCodeProp.value = "create";
    modalState.setModalState();
};

const handlerUpdateModal = (item) => {
    itemCodeProp.value = item;
    modalState.setModalState();
};
</script>

<style lang="scss" scoped>
table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0px 0px 0px;
    font-size: 18px;
    text-align: left;

    th,
    td {
        padding: 8px;
        text-align: left;
        border-bottom: 1px solid #ddd;
        text-align: center;
    }

    th {
        background-color: #2676bf;
        color: #ddd;
    }

    /* 테이블 올렸을 때 */
    tbody tr:hover {
        background-color: #d3d3d3;
        opacity: 0.9;
        cursor: pointer;
    }
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
</style>
