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
                    <a-row justify="end" style="margin-bottom: 1rem; margin-top: 1rem;">
                        <a-col :xxl="3" :xl="4" :lg="5" :md="6" :sm="6" :xs="8">
                            <sdButton type="primary" size="md">Thêm mới</sdButton>
                        </a-col>
                    </a-row>
                    <a-row>
                        <a-col :xxl="24" :xl="24" :lg="24" :md="24" :sm="24" :xs="24">
                            <a-form-item name="study-program">
                                <TableWrapper>
                                    <a-table :columns="studyProgramTableHeader" :data-source="testData"
                                        class="study-program-table" :pagination="false">
                                        <template #bodyCell="{ column, record }">
                                            <template v-if="column.key === 'thaoTac'">
                                                <div class="table-body-item study-program-operation">
                                                    <div class="program-operation-icon program-operation-edit"
                                                        :id="`edit-program-${record.id}`"
                                                        @click="">
                                                        <unicon name="edit"></unicon>
                                                    </div>
                                                    <div class="program-operation-icon program-operation-delete"
                                                        :id="`delete-program-${record.id}`"
                                                        @click="">
                                                        <unicon name="trash-alt"></unicon>
                                                    </div>
                                                </div>
                                            </template>
                                        </template>
                                    </a-table>
                                </TableWrapper>
                            </a-form-item>
                        </a-col>
                    </a-row>
                    <a-row style="margin-top: 1rem;">
                        <a-col :xxl="24" :xl="24" :lg="24" :md="24" :sm="24" :xs="24">
                            <h3>Lộ trình khoá học</h3>
                        </a-col>
                    </a-row>
                    <a-row justify="space-between">
                        <a-col :xxl="18" :xl="18" :lg="18" :md="18" :sm="18" :xs="24">
                            <a-row :gutter="12" class="add-subject-input">
                                <a-col :xxl="16" :xl="16" :lg="16" :md="16" :sm="16" :xs="24">
                                    <a-form-item name="add-subject-name" label="Môn">
                                        <a-input v-model:value="formState.createSubjectName" />
                                    </a-form-item>
                                </a-col>
                                <a-col :xxl="8" :xl="8" :lg="8" :md="8" :sm="8" :xs="24">
                                    <a-form-item name="add-subject-order" label="Số thứ tự học">
                                        <a-input v-model:value="formState.createSubjectOrder" />
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
                                    <a-table :columns="subjectTableHeader" :data-source="displayedData"
                                        class="subject-table" :pagination="false">
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
const subjectTableHeader = [
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
const studyProgramTableHeader = [
    {
        title: "Tên chương trình",
        dataIndex: "name",
        key: "name",
    },
    {
        title: "Thứ tự",
        dataIndex: "order",
        key: "order",
    },
    {
        title: "Giá",
        dataIndex: "price",
        key: "price",
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
const courseData = [
    {
        id: 1,
        trungTamId: 1,
        tenKhoa: "Fullstack .NET Web Developer",
        tenMonHoc: [
            "C Basic",
            "C Array",
            "C Function",
            "C# Basic",
            "C# Collection - Method",
            "C# OOP Basic",
            "C# OOP List Processing",
            "MS SQL Server Query",
            "MS SQL Server NonQuery",
            "EF Core Basic",
            "Web API",
            "HTML CSS",
            "Boostrap",
            "VueJS Basic",
            "VueJS Instant & Component",
            "VueJS Form & Routing & Axios",
            "VueJS Vuetify"
        ],
        kyHieu: "FN",
        hienThi: false
    },
    {
        id: 2,
        trungTamId: 3,
        tenKhoa: "Fullstack Java Web Developer",
        tenMonHoc: [
            "C Basic",
            "C Array",
            "C Function",
            "Java Core",
            "Java Collection",
            "Java Methods",
            "Java OOP Basic",
            "MySQL Query",
            "MySQL NonQuery",
            "JPA Basic",
            "Spring Boot Web Service",
            "HTML CSS",
            "Boostrap",
            "VueJS Basic",
            "VueJS Instant & Component",
            "VueJS Form & Routing & Axios",
            "VueJS Vuetify"
        ],
        kyHieu: "FJ",
        hienThi: false
    },
    {
        id: 3,
        trungTamId: 2,
        tenKhoa: "Fresher Tester",
        tenMonHoc: [
            "TF_C1_Testing Basic",
            "TF_C2_Kỹ thuật kiểm thử",
            "TF_C3_Biểu mẫu kiểm thử",
            "TF_C5_Tạo test case cho Web",
            "TF_C4_Create Test Design",
            "TF_C6_Review Test case",
            "TF_C7_Kiểm thử bảo mật",
            "TF_C8_Execute Test",
            "TF_C9_Kiểm thử ứng dụng",
            "TF_C10_Thực hành Test App",
            "TF_C11_Hướng dẫn CSDL",
            "TF_C12_Kiểm thử API",
            "TF_C13_Kiểm thử Game ",
            "MS SQL Server Query",
            "TF_C14_Tổng kết khóa"
        ],
        kyHieu: "STF",
        hienThi: false
    },
    {
        id: 4,
        trungTamId: 2,
        tenKhoa: "ISTQB Foundation",
        tenMonHoc: [
            "Found_C0_ISTQB Introduction",
            "Found_C1_FUNDAMENTALS OF TESTING",
            "Found_C2_TESTING IN THE LIFECYCLE",
            "Found_C3_STATIC TECHNIQUES",
            "FOUND_C4: TEST TECHNIQUES",
            "FOUND_C5: TEST MANAGEMENT",
            "FOUND_C6: TOOL SUPPORT FOR TESTING",
            "FOUND_MOCK EXAM",
            "FOUND_DL1",
            "FOUND_DL2",
            "FOUND_DL3",
            "FOUND_DL4",
            "FOUND_DE 1",
            "FOUND_DE 2",
            "FOUND_DE 3",
            "FOUND_DE 4",
            "FOUND_DE 5",
            "FOUND_DE 6",
            "FOUND_DE 9",
            "FOUND_DE 7",
            "FOUND_DE 8",
            "FOUND_DE 9",
            "FOUND_DE 10",
            "FOUND_DE 11"
        ],
        kyHieu: "Found",
        hienThi: false
    },
    {
        id: 5,
        trungTamId: 1,
        tenKhoa: "ISTQB Advanced Module Test Manager",
        tenMonHoc: [
            "TM_C1_Test Process",
            "TM_C2_Test management",
            "TM_C3_Reviews ",
            "TM_C4_Defect Management ",
            "TM_C5_Improving the Testing Process",
            "TM_C6_Test Tools and Automation",
            "TM_C7_People Skills – Team Composition ",
            "TM_De luyen\t1",
            "TM_De luyen\t2",
            "TM_De luyen\t3",
            "TM_De luyen\t5",
            "TM_De luyen\t6",
            "TM_De luyen\t7"
        ],
        kyHieu: "TM",
        hienThi: false
    },
    {
        id: 6,
        trungTamId: 1,
        tenKhoa: "Test Automation Developer",
        tenMonHoc: [
            "C Basic",
            "C Array",
            "C Function",
            "Java Core",
            "Java OOP Basic",
            "Selenium Framework",
            "Rest Assured API",
            "MySQL Query",
            "MySQL NonQuery",
            "JDBC"
        ],
        kyHieu: "TAD",
        hienThi: false
    },
    {
        id: 7,
        trungTamId: 1,
        tenKhoa: "Lập trình C++",
        tenMonHoc: [
            "C++ Cơ bản",
            "C++ Mảng và Collection",
            "C++ Hàm - Con trỏ",
            "C++ Cấu trúc và xử lý File",
            "C++ Xử lý chuỗi, Các thử viện mở rộng",
            "C++ OOP cơ bản",
            "C++ OOP nâng cao"
        ],
        kyHieu: "D_CPP",
        hienThi: false
    },
    {
        id: 8,
        trungTamId: 1,
        tenKhoa: "Frontend Developer",
        tenMonHoc: [
            "HTML CSS",
            "Boostrap",
            "VueJS Basic",
            "VueJS Instant & Component",
            "VueJS Form & Routing & Axios",
            "VueJS Vuetify"
        ],
        kyHieu: "FD",
        hienThi: false
    },
    {
        id: 9,
        trungTamId: 1,
        tenKhoa: "Lập trình C# Winform",
        tenMonHoc: [
            "C Basic",
            "C Array",
            "C Function",
            "C# Basic",
            "C# Collection - Method",
            "C# OOP Basic",
            "C# OOP List Processing",
            "MS SQL Server Query",
            "MS SQL Server NonQuery",
            "Winform Cơ bản",
            "Winform Nâng cao"
        ],
        kyHieu: "CWF",
        hienThi: false
    },
]
const studyProgramData = [
    {
        id: 1,
        data: [
            {
                id: 1,
                order: 1,
                name: 'Học thử miễn phí',
                price: 0
            },
            {
                id: 2,
                order: 2,
                name: 'Học xong phỏng vấn',
                price: 4
            },
        ]
    },
    {
        id: 2,
        data: [
            {
                id: 1,
                order: 1,
                name: 'Học thử miễn phí',
                price: 0
            },
        ]
    },
    {
        id: 3,
        data: [
            {
                id: 1,
                order: 1,
                name: 'Học thử miễn phí',
                price: 0
            },
        ]
    },
    {
        id: 4,
        data: [
            {
                id: 3,
                order: 1,
                name: 'ISTQB Foundation',
                price: 2700000
            },
            {
                id: 4,
                order: 2,
                name: 'ISTQB Foundation + Test tự động Selenium Advanced',
                price: 3700000
            },
            {
                id: 1,
                order: 3,
                name: 'Học thử miễn phí',
                price: 0
            },
        ]
    },
    {
        id: 5,
        data: [
            {
                id: 5,
                order: 1,
                name: 'ISTQB Advanced - Test Manager',
                price: 3500000
            },
            {
                id: 6,
                order: 2,
                name: 'ISTQB Advanced - Test Analyst + Technical Test Analyst',
                price: 3500000
            },
            {
                id: 7,
                order: 3,
                name: 'ISTQB full 3 modules Advanced: TM, TA, TTA',
                price: 6000000
            },
            {
                id: 8,
                order: 4,
                name: 'ISTQB Advanced bất kỳ + Selenium Advanced',
                price: 4500000
            },
            {
                id: 1,
                order: 5,
                name: 'Học thử miễn phí',
                price: 0
            },
        ]
    },
    {
        id: 6,
        data: [
            {
                id: 1,
                order: 1,
                name: 'Học thử miễn phí',
                price: 0
            },
            {
                id: 9,
                order: 2,
                name: 'Học xong là xong',
                price: 6000000
            },
            {
                id: 10,
                order: 2,
                name: 'Học dự án thật',
                price: 8000000
            },
        ]
    },
    {
        id: 7,
        data: [
            {
                id: 1,
                order: 1,
                name: 'Học thử miễn phí',
                price: 0
            },
            {
                id: 9,
                order: 2,
                name: 'Học xong là xong',
                price: 2500000
            },
        ]
    },
    {
        id: 8,
        data: [
            {
                id: 1,
                order: 1,
                name: 'Học thử miễn phí',
                price: 0
            },
            {
                id: 9,
                order: 2,
                name: 'Học xong là xong',
                price: 7000000
            },
        ]
    },
    {
        id: 9,
        data: [
            {
                id: 1,
                order: 1,
                name: 'Học thử miễn phí',
                price: 0
            },
            {
                id: 10,
                order: 2,
                name: 'Học dự án thật',
                price: 8000000
            },
        ]
    },
]

const testData = studyProgramData.find(program => program.id === 1)?.data
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
:global(.study-program-table .ant-table-cell) {
    text-align: center !important;
}
.table-body-item.study-program-operation {
    display: flex;
    justify-content: center;
}
.subject-operation-icon.subject-operation-delete {
    cursor: pointer;
}
.program-operation-icon.program-operation-delete, .program-operation-icon.program-operation-edit {
    cursor: pointer;
    margin: 4px;
}
:global(.subject-operation-delete > div > svg) {
    fill: rgb(247, 92, 92) !important;
}
:global(.program-operation-delete > div > svg) {
    fill: rgb(247, 92, 92) !important;
}
:global(.program-operation-edit > div > svg) {
    fill: #8231d3 !important;
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