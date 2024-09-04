<template>
    <div class="divNoticeList">
        현재 페이지: {{ cPage }} 총 개수: {{ noticeList?.listCount }}
        <table>
            <colgroup>
                <col width="10%" />
                <col width="25%" />
                <col width="10%" />
                <col width="10%" />
                <col width="25%" />
                <col width="10%" />
                <col width="10%" />
            </colgroup>

            <thead>
                <tr>
                    <th scope="col">반품 신청일자</th>
                    <th scope="col">배송시작일</th>
                    <th scope="col">배송담당자</th>
                    <th scope="col">출발지</th>
                    <th scope="col">목적지</th>
                    <th scope="col">배송개수</th>
                    <th scope="col">배송상태</th>
                </tr>
            </thead>
            <tbody>
                <template v-if="deliveryList">
                    <template v-if="deliveryList.listCount > 0">
                        <!-- v-bind:key="" 또는 :key=""를 사용하여 속성의 속성값을 바꿀 수 있다. (v-bind:속성="값" 또는 :속성="값") -->
                        <tr v-for="(item, index) in deliveryList.deliveryList" :key="index">
                            <td>{{ item.delivery_num }}</td>
                            <td>{{ item.delivery_date }}</td>
                            <td>{{ item.delivery_name }}</td>
                            <td>{{ item.delivery_start_loc }}</td>
                            <td>{{ item.delivery_end_loc }}</td>
                            <td>{{ item.obtain_count }}</td>
                            <td>{{ item.delivery_state }}</td>
                        </tr>
                    </template>
                    <template v-else>
                        <tr>
                            <td colspan="5">일치하는 검색 결과가 없습니다</td>
                        </tr>
                    </template>
                </template>
            </tbody>
        </table>
        <Pagination
            :totalItems="deliveryList?.listCount"
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

const route = useRoute();
const deliveryList = ref();
const cPage = ref(1);

const searchList = () => {
    let param = new URLSearchParams({
        cpage: cPage.value,
        pageSize: 5,
        searchTitle: route.query.searchTitle || "",
        searchStDate: route.query.searchStDate || "",
        searchEdDate: route.query.searchEdDate || ""
    });

    axios.post("/api/direction/deliveryListJson.do", param).then((res) => {
        deliveryList.value = res.data;
    });
};

watch(route, searchList);

onMounted(() => {
    searchList();
});
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
</style>
