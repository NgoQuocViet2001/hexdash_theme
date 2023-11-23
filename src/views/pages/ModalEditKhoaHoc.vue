<template>
    <sdModal title="Sửa khoá học" :type="type" :visible="visible" :onOk="onOk" :onCancel="onCancel" width="60rem"
        :bodyStyle="{ overflowY: 'auto', maxHeight: '90vh' }">
        <div class="create-course">
            <BasicFormWrapper>
                <a-form name="course" layout="vertical">
                    <a-row :gutter="12">
                        <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="24" :xs="24">
                            <a-form-item name="avatar-preview" label="Ảnh đại diện">
                                <img :src="formState.avatarPreview" alt="Avatar Preview"
                                    style="height: 18rem; width: 100%; object-fit: cover;" />
                            </a-form-item>
                        </a-col>
                        <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="24" :xs="24">
                            <a-form-item name="illustration-preview" label="Ảnh minh hoạ">
                                <img :src="formState.illustrationPreview" alt="Illustration Preview"
                                    style="height: 18rem; width: 100%; object-fit: cover;" />
                            </a-form-item>
                        </a-col>
                    </a-row>
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
                                            :action="formState.apiUpload" @change="handleAvatarChange">
                                            <sdButton type="primary">
                                                <img :src="'/src/assets/img/icon/image.png'" alt="" />
                                                <span>Sửa ảnh đại diện</span>
                                            </sdButton>
                                        </a-upload>
                                    </a-form-item>
                                </a-col>
                                <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="24" :xs="24">
                                    <a-form-item name="illustration">
                                        <a-upload v-model:file-list="formState.illustration" :max-count="1"
                                            :action="formState.apiUpload" @change="handleIllustrationChange">
                                            <sdButton type="primary">
                                                <img :src="'/src/assets/img/icon/image.png'" alt="" />
                                                <span>Sửa ảnh minh hoạ</span>
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
                    <a-row justify="end" style="margin-bottom: 10px;">
                        <a-col :xxl="3" :xl="4" :lg="5" :md="6" :sm="6" :xs="8">
                            <sdButton type="primary">Thêm mới</sdButton>
                        </a-col>
                    </a-row>
                    <a-row>
                        <a-col :xxl="24" :xl="24" :lg="24" :md="24" :sm="24" :xs="24"><h3>Lộ trình khoá học</h3></a-col>
                    </a-row>
                    <a-row justify="space-between">
                        <a-col :xxl="18" :xl="18" :lg="18" :md="18" :sm="18" :xs="24">
                            <a-row :gutter="12" class="add-subject-input">
                                <a-col :xxl="16" :xl="16" :lg="16" :md="16" :sm="16" :xs="24">
                                    <a-form-item name="add-subject-name" label="Môn">
                                        <a-input />
                                    </a-form-item>
                                </a-col>
                                <a-col :xxl="8" :xl="8" :lg="8" :md="8" :sm="8" :xs="24">
                                    <a-form-item name="add-subject-order" label="Số thứ tự học">
                                        <a-input />
                                    </a-form-item>
                                </a-col>
                            </a-row>
                        </a-col>
                        <a-col :xxl="3" :xl="3" :lg="3" :md="3" :sm="4" :xs="8" class="add-subject-action-container">
                            <a-row :gutter="8" class="add-subject-action" style=" margin-bottom: 20px;">
                                <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="12" :xs="12">
                                    <div class="add-subject-action-icon icon-subject-add">
                                        <img src="https://cdn-icons-png.flaticon.com/512/1427/1427091.png" alt="add"
                                            style="width: 3rem; height: 3rem; object-fit: cover;" />
                                    </div>
                                </a-col>
                                <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="12" :xs="12">
                                    <div class="add-subject-action-icon icon-subject-refresh">
                                        <img src="https://cdn-icons-png.flaticon.com/512/189/189687.png" alt="refresh"
                                            style="width: 3rem; height: 3rem; object-fit: cover;" />
                                    </div>
                                </a-col>
                            </a-row>
                        </a-col>
                    </a-row>
                    <a-row>
                        <a-col :xxl="24" :xl="24" :lg="24" :md="24" :sm="24" :xs="24">
                            <a-form-item name="subjects">
                                <TableWrapper>
                                    <a-table :columns="tableHeader" :data-source="displayedData" class="subject-table"
                                        :pagination="false">
                                        <template #bodyCell="{ column, record }">
                                            <template v-if="column.key === 'thaoTac'">
                                                <div class="subject-operation-icon subject-operation-delete"
                                                    :id="`delete-subject-${record.id}`"
                                                    @click="deleteSubjectInCourse(courseId, record.id)">
                                                    <unicon name="trash-alt"></unicon>
                                                </div>
                                            </template>
                                        </template>
                                    </a-table>
                                </TableWrapper>
                            </a-form-item>

                        </a-col>
                        <a-col :xxl="24" :xl="24" :lg="24" :md="24" :sm="24" :xs="24">
                            <div class="pagination-wrapper pagination-wrapper-subject">
                                <a-pagination v-model="currentPage" :total="courseSubjectTotal" :pageSize="pageSize"
                                    show-less-items @change="handlePageChange" />
                            </div>
                        </a-col>
                    </a-row>
                    <a-row justify="end">
                        <a-col :xxl="4" :xl="5" :lg="6" :md="8" :sm="10" :xs="10">
                            <sdButton type="primary" size="lg" @click="onOk">Cập nhật</sdButton>
                        </a-col>
                    </a-row>
                </a-form>
            </BasicFormWrapper>
        </div>
    </sdModal>
</template>
<script setup lang="ts">
import { BasicFormWrapper } from '@/views/styled';
import { ref, computed, onMounted } from 'vue';
import { TableWrapper } from "@/views/styled";
const currentPage = ref(1);
const pageSize = ref(5);
const handlePageChange = (page: any) => {
    currentPage.value = page;
}
const props = defineProps([
    'courseId',
    'type',
    'visible',
    'onOk',
    'onCancel',
    'formState',
    'courseSubjects',
    'courseSubjectTotal',
    'deleteSubjectInCourse'
]);

const displayedData = computed(() => {
    const start = (currentPage.value - 1) * pageSize.value;
    const end = currentPage.value * pageSize.value;
    return props.courseSubjects?.slice(start, end);
})
const tableHeader = [
    {
        title: "Môn học",
        dataIndex: "name",
        key: "name",
    },
    {
        title: "Thứ tự",
        dataIndex: "order",
        key: "order",
    },
    {
        title: "Thao tác",
        key: "thaoTac",
    },
]
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
const onCancel = () => {
    currentPage.value = 1;
    props.onCancel();
}
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

:global(body > div:nth-child(11) > div > div.ant-modal-wrap > div > div.ant-modal-content > div.ant-modal-body > div > div > form > div.ant-row.ant-row-end > div > button) {
    width: 100% !important;
}

:global(.subject-table .ant-table-cell) {
    text-align: center !important;
}

.subject-operation-icon.subject-operation-delete {
    cursor: pointer;
}

:global(.subject-operation-delete > div > svg) {
    fill: rgb(247, 92, 92) !important;
}

.add-subject-action-container {
    display: flex;
    align-items: center;
    transform: translateY(1rem);
}
.add-subject-action-icon:hover {
    cursor: pointer;
    opacity: 0.85;
}
.ant-table-wrapper.subject-table {
    height: 22rem;
}


.pagination-wrapper.pagination-wrapper-subject {
    display: flex;
    justify-content: center;
}
</style>