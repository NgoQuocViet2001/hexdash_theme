<template>
    <div >
        <sdModal :title="currentModal?.label" :type="type" :visible="visible" :onOk="onOk" :onCancel="onCancel" :bodyStyle="{overflowY: 'auto', maxHeight: '92vh'}">
        <div class="project-modal">
            <BasicFormWrapper>
                <div class="create-challenge-form">
                    <a-form name="challenge" layout="vertical">
                        <a-form-item v-if="currentModal?.name" name="challengeName" :label="currentModal?.name">
                            <a-input v-model:value="formState.challengeName"/>
                        </a-form-item>
                        <a-form-item v-if="currentModal?.banner" name="challengeBanner" :label="currentModal?.banner">
                            <a-upload :action="formState.apiUpload" name="challengeUpload" v-model:file-list="formState.bannerFile" :max-count="1">
                                <a-button>
                                    <unicon name="upload"></unicon>
                                </a-button>
                            </a-upload>
                        </a-form-item>
                        <a-form-item v-if="currentModal?.description" name="challengeBanner" :label="currentModal?.description">
                            <a-textarea :rows="3" placeholder="Mô tả ngắn" v-model:value="formState.shortDesc"/>
                        </a-form-item>
                        <a-form-item v-if="currentModal?.attendCount" name="challengeAttendCount"
                            :label="currentModal?.attendCount">
                            <a-input />
                        </a-form-item>
                        <a-row :gutter="20">
                            <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="12" :xs="24">
                                <a-form-item v-if="currentModal?.start" name="challengeStart" :label="currentModal?.start">
                                    <a-date-picker v-model:value="formState.start" placeholder="mm/dd/yyyy"
                                        :format="dateFormat" />
                                </a-form-item>
                            </a-col>
                            <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="12" :xs="24">
                                <a-form-item v-if="currentModal?.end" name="challengeEnd" :label="currentModal?.end">
                                    <a-date-picker v-model:value="formState.end" placeholder="mm/dd/yyyy"
                                        :format="dateFormat" />
                                </a-form-item>
                            </a-col>
                        </a-row>
                        <a-row :gutter="20">
                            <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="12" :xs="24">
                                <a-form-item v-if="currentModal?.level" name="challengeLevel" :label="currentModal?.level">
                                    <a-select v-model:value="formState.level" style="width: 100%">
                                        <a-select-option value="easy">Dễ</a-select-option>
                                        <a-select-option value="medium">Trung bình</a-select-option>
                                        <a-select-option value="hard">Khó</a-select-option>
                                    </a-select>
                                </a-form-item>
                            </a-col>
                            <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="12" :xs="24">
                                <a-form-item v-if="currentModal?.challengeTime" name="challengeTime"
                                    :label="currentModal?.challengeTime">
                                    <a-select v-model:value="formState.challengeTime" style="width: 100%" >
                                        <a-select-option value="time1">30 phút</a-select-option>
                                        <a-select-option value="time2">60 tiếng</a-select-option>
                                        <a-select-option value="time3">90 phút</a-select-option>
                                        <a-select-option value="time4">120 phút</a-select-option>
                                    </a-select>
                                </a-form-item>
                            </a-col>
                        </a-row>

                        <a-row>
                            <sdButton type="primary" size="lg" @click="onOk">{{ currentModal?.label }}</sdButton>
                        </a-row>
                    </a-form>
                </div>
            </BasicFormWrapper>
        </div>
    </sdModal>
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { BasicFormWrapper } from '@/views/styled';
const dateFormat = 'MM/DD/YYYY';
const props = defineProps([
    'visible',
    'onOk',
    'onCancel',
    'modalID',
    'type',
    'formState',
]);
const currentModal = computed(() => modalText.find(item => item.id === props.modalID));

const modalText = [
    {
        id: 1,
        label: 'Tạo giải đấu',
        name: 'Tên giải đấu',
        banner: 'Banner giải đấu',
        description: 'Mô tả ngắn',
        uploadBanner: 'Upload',
        attendCount: 'Giới hạn người tham gia',
        start: 'Bắt đầu lúc',
        end: 'Kết thúc lúc',
        level: 'Độ khó',
        challengeTime: 'Thời gian làm bài',
    },
    {
        id: 2,
        label: 'Tạo trận so tài',
        name: 'Tên trận so tài',
        challengeTime: 'Thời gian làm bài',
        level: 'Độ khó',
    },
    {
        id: 3,
        label: 'Tạo trận luyện tập',
        name: 'Tên trận luyện tập',
        challengeTime: 'Thời gian làm bài',
        level: 'Độ khó',
    },
]

</script>

<style scoped>
/* css modal */
:global(.ant-modal-wrap) {
    display: flex;
    align-items: center;
    justify-content: center;
}
:global(.ant-modal) {
    top: 0;
    padding-bottom: 0;
}
.ant-picker {
    width: 100%;
}
</style>