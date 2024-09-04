<template>
    <div class="divNoticeList">
        현재 페이지: {{ cPage }} 총 개수: {{ noticeList?.listCount }}
        <table>
            <colgroup>
                <col width="30%" />
                <col width="15%" />
                <col width="10%" />
                <col width="35%" />
                <col width="10%" />
            </colgroup>

            <thead>
                <tr>
                    <th scope="col">반품 신청일자</th>
                    <th scope="col">제품명</th>
                    <th scope="col">반품개수</th>
                    <th scope="col">금액</th>
                    <th scope="col">반품상태</th>
                </tr>
            </thead>
            <tbody>
                <template v-if="returnList">
                    <template v-if="returnList.listCount > 0">
                        <!-- v-bind:key="" 또는 :key=""를 사용하여 속성의 속성값을 바꿀 수 있다. (v-bind:속성="값" 또는 :속성="값") -->
                        <tr v-for="(item, index) in returnList.returnList" :key="index">
                            <td>{{ item.return_order_date }}</td>
                            <td>{{ item.item_name }}</td>
                            <td>{{ item.return_count }}</td>
                            <td>{{ item.return_price }}</td>
                            <td>{{ item.signYN }}</td>
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
            :totalItems="returnList?.listCount"
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
const returnList = ref();
const cPage = ref(1);

const searchList = () => {
    let param = new URLSearchParams({
        cpage: cPage.value,
        pageSize: 5,
        searchTitle: route.query.searchTitle || "",
        searchStDate: route.query.searchStDate || "",
        searchEdDate: route.query.searchEdDate || ""
    });

    axios.post("/api/direction/returnListJson.do", param).then((res) => {
        returnList.value = res.data;
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
