<template>
    <table>
        <colgroup>
            <col width="10%" />
            <col width="5%" />
            <col width="20%" />
            <col width="5%" />
            <col width="5%" />
            <col width="10%" />
            <col width="10%" />
            <col width="10%" />
            <col width="5%" />
        </colgroup>
        <thead>
            <tr>
                <th>고객명</th>
                <th>제품명</th>
                <th>주소</th>
                <th>개수</th>
                <th>가격</th>
                <th>총 가격</th>
                <th>반품 신청 일자</th>
                <th>반풀 처리 일자</th>
                <th>승인여부</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="order in orderObj.returnList" :key="order.seq">
                <td :class="order.signYN === 'Y' ? 'blue' : 'red'">
                    {{ order.cust_name }}{{ order.signYN === "Y" ? "(승인)" : "(미승인)" }}
                </td>
                <td>{{ order.item_name }}</td>
                <td>{{ order.adr }}</td>
                <td>{{ order.return_count }}</td>
                <td>{{ order.item_price.toLocaleString() }} 원</td>
                <td>{{ order.sum != null ? parseInt(order.sum).toLocaleString() : "" }} 원</td>
                <td>{{ order.return_order_date }}</td>
                <td>{{ order.return_processing_date }}</td>
                <td>
                    <button v-if="order.signYN === 'N'" @click="clickProp(order.seq)">승인</button>
                    <button v-else disabled>승인 완료</button>
                </td>
            </tr>
        </tbody>
    </table>
    <Pagination
        v-if="orderObj.returnListCount > pageSize"
        :totalItems="orderObj?.returnListCnt || 0"
        :itemsPerPage="pageSize"
        :maxPagesShown="5"
        :onClick="viewList"
        v-model="cPage"
    />
</template>

<script setup>
import Pagination from "@/components/common/Pagination.vue";
import axios from "axios";
import { useModalStore } from "@/stores/modalState";

const cPage = ref(1);
const pageSize = ref(10);

const orderObj = ref(new Object());
const injectedValue = inject("returnProvidedValue");

const modalState = useModalStore();
const injectedModalProp = inject("modalProp");

const viewList = async () => {
    await axios
        .post("/api/executives/returnApprovalJson", {
            cpage: cPage.value,
            pageSize: pageSize.value,
            signYN: injectedValue.value
        })
        .then((res) => {
            orderObj.value = res.data;
            console.log("order obj in return");
            console.log(orderObj.value);
        });
};

const clickProp = (seq) => {
    injectedModalProp.value = {
        str: "tb_return",
        seq: seq,
        state: false
    };
    modalState.setModalState();
    viewList();
};

watch(
    () => injectedModalProp.value.state,
    (newState) => {
        if (newState === true) { // false로 변경될 때
            console.log("Modal closed, reloading list");
            viewList(); // 모달이 닫힐 때 viewList 호출
        }
    }
);

onMounted(() => {
    viewList();
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
        border: 1px solid #ddd;
        text-align: center;
        height: 37px;
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

a {
    cursor: pointer;
    &:hover {
        text-decoration: underline;
        color: white;
    }
}

button {
    text-align: center;
    text-decoration: none;
    display: block;
    border: none;
    color: white;
    width: 80px;
    padding-top: 10px;
    padding-bottom: 10px;
    font-size: 12px;
    margin: 10px 2px;
    cursor: pointer;
    border-radius: 12px;
    box-shadow: 0 4px #999;
    background-color: #3bb2ea;
    float: inline-end;

    &:hover {
        background-color: #45a049;
    }

    &:active {
        background-color: #3e8e41;
        box-shadow: 0 2px #666;
        transform: translateY(2px);
    }
}

.blue {
    color: blue; /* 글자색을 흰색으로 설정 */
}

.red {
    color: red; /* 글자색을 흰색으로 설정 */
}

button:disabled {
    background-color: light-dark(rgba(239, 239, 239, 0.3), rgba(19, 1, 1, 0.1));
    color: black;
    border-color: light-dark(rgba(118, 118, 118, 0.3), rgba(195, 195, 195, 0.3));
}

.orderDetailBtn {
    float: none;
    margin: 0 0;
    display: inline-block;
}
</style>
