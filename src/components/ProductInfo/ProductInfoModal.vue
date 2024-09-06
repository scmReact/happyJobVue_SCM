<template>
    <teleport to="body">
        <div class="backdrop">
            <div class="container">
                <div>
                    <div v-if="imageUrl">
                        <label>미리보기</label>
                        <img :src="imageUrl" />
                    </div>
                </div>
                <div v-if="itemCodeProp === 'create'">
                    <label> 제품코드 :<input type="text" v-model="insertData.value.item_code" /> </label>
                    <label> 제품명 :<input type="text" v-model="insertData.value.item_name" /> </label>
                    <label> 제조사 :<input type="text" v-model="insertData.value.manufac" /> </label>
                    <label> 제품가격 :<input type="text" v-model="insertData.value.provide_value" /> </label>
                    <label> 상세정보 :<input type="text" v-model="insertData.value.product_detail" /> </label>
                </div>
                <div v-else>
                    <label> 제품코드 :<input type="text" v-model="itemCodeProp.item_code" /> </label>
                    <label> 제품명 :<input type="text" v-model="itemCodeProp.item_name" /> </label>
                    <label> 제조사 :<input type="text" v-model="itemCodeProp.manufac" /> </label>
                    <label> 제품가격 :<input type="text" v-model="itemCodeProp.provide_value" /> </label>
                    <label> 상세정보 :<input type="text" v-model="itemCodeProp.product_detail" /> </label>
                </div>

                <div class="button-box">
                    <button @click="itemCodeProp === 'create' ? productSave() : productUpdate()">
                        {{ itemCodeProp === "create" ? "저장" : "수정" }}
                    </button>
                    <button v-if="itemCodeProp.item_code > 0" @click="productDelete()">삭제</button>
                    <button @click="modalState.setModalState">나가기</button>
                </div>
            </div>
        </div>
    </teleport>
</template>

<script setup>
import { useModalStore } from "@/stores/modalState";
import axios from "axios";

// 부모 컴포넌트에서 전달받은 props를 사용할 수 있도록 정의한다.
const { itemCodeProp } = defineProps(["itemCodeProp"]);

const modalState = useModalStore();
const imageUrl = ref("");
const fileData = ref("");
const insertData = {
    value: { item_code: "", item_name: "", manufac: "", provide_value: "", product_detail: "" }
};

const productSave = () => {
    console.log(insertData.value);
    axios
        .post("/api/management/productRegistJson.do", {
            product_code: insertData.value.item_code,
            product_name: insertData.value.item_name,
            manufac: insertData.value.manufac,
            product_value: insertData.value.provide_value,
            product_detail: insertData.value.product_detail
        })

        .then((res) => {
            if (res.data.result === "success") {
                modalState.setModalState();
                emit("postSuccess");
            }
        });
};

const productUpdate = () => {
    axios
        .post("/api/management/productUpdateJson.do", {
            item_code: itemCodeProp.item_code,
            product_code: itemCodeProp.item_code,
            product_name: itemCodeProp.item_name,
            manufac: itemCodeProp.manufac,
            product_value: itemCodeProp.provide_value,
            product_detail: itemCodeProp.product_detail
        })
        .then((res) => {
            if (res.data.result === "success") {
                modalState.setModalState();
                emit("postSuccess");
            }
        });
};

const productDelete = () => {
    axios
        .post("/api/management/productDelete.do", {
            product_code: itemCodeProp.item_code
        })
        .then((res) => {
            if (res.data.result === "success") {
                modalState.setModalState();
                emit("postSuccess");
            }
        });
};
</script>

<style lang="scss" scoped>
.backdrop {
    top: 0%;
    left: 0%;
    width: 100%;
    height: 100%;
    position: fixed;
    display: flex;
    flex-flow: row wrep;
    justify-content: center;
    align-items: center;
    background: rgba(0, 0, 0, 0.5);
    z-index: 1;
    font-weight: bold;
}

label {
    display: flex;
    flex-direction: column;
}

input[type="text"] {
    padding: 8px;
    margin-top: 5px;
    margin-bottom: 5px;
    border-radius: 4px;
    border: 1px solid #ccc;
}

.container {
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.25);
    position: relative;
    width: 400px;
}

img {
    width: 100px;
    height: 100px;
}

.img-label {
    margin-top: 10px;
    padding: 6px 25px;
    background-color: #ccc;
    border-radius: 4px;
    color: rgba(0, 0, 0, 0.9);
    cursor: pointer;

    &:hover {
        background-color: #45a049;
        color: white;
    }

    &:active {
        background-color: #3e8e41;
        box-shadow: 0 2px #666;
        transform: translateY(2px);
    }
}

.button-box {
    text-align: right;
    margin-top: 10px;
}
button {
    background-color: #3bb2ea;
    border: none;
    color: white;
    padding: 10px 22px;
    text-align: right;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    border-radius: 12px;
    box-shadow: 0 4px #999;
    transition: 0.3s;

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
