<template>
    <sdModal title="Thêm khoá học" :type="type" :visible="visible" :onOk="onOk" :onCancel="onCancel" width="60rem">
        <div class="create-course">
            <BasicFormWrapper>
                <a-form name="course" layout="vertical">
                    <a-row :gutter="20">
                        <a-col :xxl="8" :xl="8" :lg="8" :md="24" :sm="24" :xs="24">
                            <a-form-item name="courseName" label="Tên khoá học">
                                <a-input v-model="formState.courseName" />
                            </a-form-item>
                        </a-col>
                        <a-col :xxl="8" :xl="8" :lg="8" :md="24" :sm="24" :xs="24">
                            <a-form-item name="courseSymbol" label="Ký hiệu">
                                <a-input v-model="formState.courseSymbol" />
                            </a-form-item>
                        </a-col>
                        <a-col :xxl="8" :xl="8" :lg="8" :md="24" :sm="24" :xs="24">
                            <a-form-item name="center" label="Trung tâm">
                                <a-select v-model:value="formState.center" style="width: 100%">
                                    <a-select-option value="center1">Lotus Academy</a-select-option>
                                    <a-select-option value="center2">Đặng Tiến Đông</a-select-option>
                                    <a-select-option value="center3">Mỹ Đình</a-select-option>
                                </a-select>
                            </a-form-item>
                        </a-col>
                    </a-row>
                    <a-row justify="space-between">
                        <a-col :xxl="4" :xl="4" :lg="6" :md="6" :sm="24" :xs="24">
                            <a-form-item name="active">
                                <a-checkbox v-model:checked="formState.active">Active</a-checkbox>
                            </a-form-item>
                        </a-col>
                        <a-col :xxl="10" :xl="10" :lg="12" :md="18" :sm="24" :xs="24">
                            <a-row :gutter="10">
                                <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="24" :xs="24">
                                    <a-form-item name="avatar">
                                        <a-upload v-model:file-list="formState.avatar" :max-count="1"
                                            action="https://www.mocky.io/v2/5cc8019d300000980a055e76">
                                            <sdButton type="primary">
                                                <img :src="'/src/assets/img/icon/image.png'" alt="" />
                                                <span>Thêm ảnh đại diện</span>
                                            </sdButton>
                                        </a-upload>
                                    </a-form-item>
                                </a-col>
                                <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="24" :xs="24">
                                    <a-form-item name="illustration">
                                        <a-upload v-model:file-list="formState.illustration" :max-count="1"
                                            action="https://www.mocky.io/v2/5cc8019d300000980a055e76">
                                            <sdButton type="primary">
                                                <img :src="'/src/assets/img/icon/image.png'" alt="" />
                                                <span>Thêm ảnh minh hoạ</span>
                                            </sdButton>
                                        </a-upload>
                                    </a-form-item>
                                </a-col>
                            </a-row>
                        </a-col>
                    </a-row>
                    <a-row>
                        <a-col :xxl="24" :xl="24" :lg="24" :md="24" :sm="24" :xs="24">
                            <a-form-item name="shortDesc" label="Mô tả ngắn">
                                <a-textarea :rows="4" placeholder="Mô tả ngắn" v-model="formState.shortDesc" />
                            </a-form-item>
                        </a-col>
                    </a-row>
                    <a-row>
                        <sdButton type="primary" size="lg" @click="onOk">Thêm khoá học</sdButton>
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
    'onOk',
    'onCancel',
    'type',
    'formState',
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
:global(.ant-upload.ant-upload-select.ant-upload-select-text) {
    width: 100% !important;
}

:global(.fEJzIv.ant-btn) {
    width: 100% !important;
}

</style>