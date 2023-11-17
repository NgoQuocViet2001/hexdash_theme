<template>
    <sdModal :title="currentModal?.label" :type="type" :visible="visible" :onOk="onOk" :onCancel="onCancel">
        <div class="project-modal">
            <BasicFormWrapper>
                <div class="create-challenge-form">
                    <a-form name="challenge" layout="vertical">
                        <a-form-item v-if="currentModal?.name" name="challengeName" :label="currentModal?.name">
                            <a-input />
                        </a-form-item>
                        <a-form-item v-if="currentModal?.banner" name="challengeBanner" :label="currentModal?.banner">
                            <a-upload action="/api/upload" name="challengeUpload" :show-upload-list="false">
                                <a-button>
                                    <unicon name="upload"></unicon>
                                </a-button>
                            </a-upload>
                        </a-form-item>
                        <a-form-item v-if="currentModal?.description" name="challengeBanner" :label="currentModal?.description">
                            <a-textarea :rows="3" placeholder="Mô tả ngắn" />
                        </a-form-item>
                        <a-form-item v-if="currentModal?.attendCount" name="challengeAttendCount"
                            :label="currentModal?.attendCount">
                            <a-input />
                        </a-form-item>
                        <a-row :gutter="60">
                            <a-col :md="12">
                                <a-form-item v-if="currentModal?.start" name="challengeStart" :label="currentModal?.start">
                                    <a-date-picker v-model:value="formState.start" placeholder="mm/dd/yyyy"
                                        :format="dateFormat" />
                                </a-form-item>
                            </a-col>
                            <a-col :md="12">
                                <a-form-item v-if="currentModal?.end" name="challengeEnd" :label="currentModal?.end">
                                    <a-date-picker v-model:value="formState.end" placeholder="mm/dd/yyyy"
                                        :format="dateFormat" />
                                </a-form-item>
                            </a-col>
                        </a-row>
                        <a-row :gutter="60">
                            <a-col :md="12">
                                <a-form-item v-if="currentModal?.level" name="challengeLevel" :label="currentModal?.level">
                                    <a-select v-model:value="formState.level" style="width: 100%">
                                        <a-select-option value="ease">Dễ</a-select-option>
                                        <a-select-option value="medium">Trung bình</a-select-option>
                                        <a-select-option value="hard">Khó</a-select-option>
                                    </a-select>
                                </a-form-item>
                            </a-col>
                            <a-col :md="12">
                                <a-form-item v-if="currentModal?.challengeTime" name="challengeTime"
                                    :label="currentModal?.challengeTime">
                                    <a-select v-model:value="formState.challengeTime" style="width: 100%">
                                        <a-select-option value="time1">30 phút</a-select-option>
                                        <a-select-option value="time2">60 tiếng</a-select-option>
                                        <a-select-option value="time3">90 phút</a-select-option>
                                        <a-select-option value="time4">120 phút</a-select-option>
                                    </a-select>
                                </a-form-item>
                            </a-col>
                        </a-row>

                        <a-row>
                            <sdButton type="primary" size="lg">{{ currentModal?.label }}</sdButton>
                        </a-row>
                    </a-form>
                </div>
            </BasicFormWrapper>
        </div>
    </sdModal>
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