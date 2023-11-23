<template>
    <sdModal title="Thêm khoá học" :type="type" :visible="visible" :onOk="onOk" :onCancel="onCancel" width="60rem" :bodyStyle="{overflowY: 'auto', maxHeight: '95vh'}">
        <div class="create-course">
            <BasicFormWrapper>
                <a-row :gutter="10">
                    <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="24" :xs="24">
                        <a-form-item v-if="avatarPreview" name="avatar-preview" label="Ảnh đại diện">
                            <img  src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQavptRqKumWD_6LEwh1eiscVM3Vhi-9sRuAw&usqp=CAU" alt="Avatar Preview"
                                style="height: 18rem; width: 100%; object-fit: cover;" />
                        </a-form-item>
                    </a-col>
                    <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="24" :xs="24">
                        <a-form-item v-if="illustrationPreview" name="illustration-preview" label="Ảnh minh hoạ">
                            <img  src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ9FQNAoZR58Cf_TtkKOfUOlTQ8KkXX9dMNdA&usqp=CAU" alt="Illustration Preview"
                                style="height: 18rem; width: 100%; object-fit: cover;" />
                        </a-form-item>
                    </a-col>
                </a-row>
                <a-form name="course" layout="vertical">
                    <a-row :gutter="20">
                        <a-col :xxl="8" :xl="8" :lg="8" :md="24" :sm="24" :xs="24">
                            <a-form-item name="courseName" label="Tên khoá học">
                                <a-input v-model:value="formState.courseName" />
                            </a-form-item>
                        </a-col>
                        <a-col :xxl="8" :xl="8" :lg="8" :md="24" :sm="24" :xs="24">
                            <a-form-item name="courseSymbol" label="Ký hiệu">
                                <a-input v-model:value="formState.courseSymbol" />
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
                                            :action="formState.apiUpload"
                                            @change="handleAvatarChange">
                                            <sdButton type="primary">
                                                <img :src="'/src/assets/img/icon/image.png'" alt="image" />
                                                <span>Thêm ảnh đại diện</span>
                                            </sdButton>
                                        </a-upload>
                                    </a-form-item>
                                </a-col>
                                <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="24" :xs="24">
                                    <a-form-item name="illustration">
                                        <a-upload v-model:file-list="formState.illustration" :max-count="1"
                                            :action="formState.apiUpload"
                                            @change="handleIllustrationChange">
                                            <sdButton type="primary">
                                                <img :src="'/src/assets/img/icon/image.png'" alt="image" />
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
                                <a-textarea :rows="4" placeholder="Mô tả ngắn" v-model:value="formState.shortDesc" />
                            </a-form-item>
                        </a-col>
                    </a-row>
                    <a-row justify="end">
                        <a-col :xxl="4" :xl="5" :lg="6" :md="8" :sm="10" :xs="10">
                            <sdButton type="primary" size="lg" @click="onOk">Thêm khoá học</sdButton>

                        </a-col>
                    </a-row>
                </a-form>
            </BasicFormWrapper>
        </div>
    </sdModal>
</template>
<script setup lang="ts">
import { BasicFormWrapper } from '@/views/styled';
import { ref } from 'vue';

const props = defineProps([
    'visible',
    'onOk',
    'onCancel',
    'type',
    'formState',
]);
const avatarPreview = ref<string | null>(null);
const illustrationPreview = ref<string | null>(null);
const handleAvatarChange = (info: any) => {
    if (info.file.status === 'done') {
        console.log(info.file.response.url);
        avatarPreview.value = info.file.response.url;

    }
};
const handleIllustrationChange = (info: any) => {
    if (info.file.status === 'done') {
        console.log(info.file.response.url);
        illustrationPreview.value = info.file.response.url;
    }
};
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
:global(body > div:nth-child(10) > div > div.ant-modal-wrap > div > div.ant-modal-content > div.ant-modal-body > div > div > form > div.ant-row.ant-row-end > div > button){
    width: 100% !important;
}
</style>