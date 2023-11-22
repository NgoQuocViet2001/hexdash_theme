<template>
    <sdModal title="Xác nhận xoá" :type="type" :visible="visible" @cancel="handleCancel" @ok="confirmDelete">
        <div class="custom-delete-modal">
            <BasicFormWrapper>
                <a-form name="delete-modal" layout="vertical">
                    <a-row style="margin-bottom: 8%;">
                        <a-col :xxl="24" :xl="24" :lg="24" :md="24" :sm="24" :xs="24">
                            <p>Bạn có chắc chắn muốn xoá?</p>
                        </a-col>
                    </a-row>
                    <a-row :gutter="10" justify="end">
                        <a-col :xxl="3" :xl="3" :lg="3" :md="3" :sm="3" :xs="3">
                            <sdButton type="light" @click="handleCancel">Huỷ</sdButton>
                        </a-col>
                        <a-col :xxl="5" :xl="5" :lg="5" :md="5" :sm="5" :xs="5">
                            <sdButton type="primary" @click="confirmDelete">Xác nhận</sdButton>
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
</style>