<template>
    <ContextBox>발주 승인</ContextBox>
    <OrderSearch />
    <OrderApproval />

    <br /><br /><br />

    <ContextBox>반품입금승인</ContextBox>
    <ReturnSearch />
    <ReturnApproval />
    <ApprovalModal v-if="modalState.modalState" :modalProp="modalProp" @reloadTable="viewList"/>
</template>

<script setup>
import ContextBox from "@/components/common/ContextBox.vue";
import OrderSearch from "@/components/page/Executives/Approval/OrderSearch.vue";
import OrderApproval from "@/components/page/Executives/Approval/OrderApproval.vue";
import ReturnSearch from "@/components/page/Executives/Approval/ReturnSearch.vue";
import ReturnApproval from "@/components/page/Executives/Approval/ReturnApproval.vue";
import ApprovalModal from "@/components/page/Executives/Approval/ApprovalModal.vue";
import { useModalStore } from "@/stores/modalState";

const modalState = useModalStore();
const modalProp = ref();
const orderProvidedValue = ref();
const returnProvidedValue = ref();

provide("modalProp", modalProp);
provide("orderProvidedValue", orderProvidedValue);
provide("returnProvidedValue", returnProvidedValue);

// modalProp에 SUCCESS 보내고 화면 리로딩

</script>
