<template>
    <sdModal title="Xác nhận xoá" :type="type" :visible="visible" @cancel="handleCancel" @ok="confirmDelete" :bodyStyle="{overflowY: 'auto', maxHeight: '95vh'}">
        <div class="custom-delete-modal">
            <BasicFormWrapper>
                <a-form name="delete-modal" layout="vertical">
                    <a-row style="margin-bottom: 8%;">
                        <a-col :xxl="24" :xl="24" :lg="24" :md="24" :sm="24" :xs="24">
                            <p>Bạn có chắc chắn muốn xoá?</p>
                        </a-col>
                    </a-row>
                    <a-row :gutter="10" justify="end">
                        <a-col :xxl="3" :xl="3" :lg="5" :md="6" :sm="6" :xs="8">
                            <sdButton type="light" @click="handleCancel">Huỷ</sdButton>
                        </a-col>
                        <a-col :xxl="5" :xl="5" :lg="7" :md="8" :sm="8" :xs="10">
                            <sdButton type="danger" @click="confirmDelete">Xác nhận</sdButton>
                        </a-col>
                    </a-row>
                </a-form>
            </BasicFormWrapper>

        </div>

    </sdModal>
</template>
<script setup lang="ts">
import { BasicFormWrapper } from '@/views/styled';

const props = defineProps([
    'visible',
    'type',
    'handleCancel',
    'confirmDelete'
]);
</script>
<style scoped>
:global(.ant-modal-wrap) {
    display: flex;
    align-items: center;
    justify-content: center;
}

:global(.ant-modal) {
    top: 0;
    padding-bottom: 0;
}
:global(body > div:nth-child(11) > div > div.ant-modal-wrap > div > div.ant-modal-content > div.ant-modal-body > div > div > form > div.ant-row.ant-row-end > div.ant-col.ant-col-xs-8.ant-col-sm-8.ant-col-md-6.ant-col-lg-5.ant-col-xl-3.ant-col-xxl-3 > button) {
    width: 100% !important;
}
</style>