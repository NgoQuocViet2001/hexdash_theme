<template>
    <div class="course-container">
        <Main>
            <BreadcrumbWrapperStyle>
                <a-breadcrumb>
                    <a-breadcrumb-item>
                        <router-link to="/">Home</router-link>
                    </a-breadcrumb-item>
                    <a-breadcrumb-item>
                        <router-link to="/noidung/khoahoc">Khoá học</router-link>
                    </a-breadcrumb-item>
                </a-breadcrumb>
            </BreadcrumbWrapperStyle>
            <div class="course-content">
                <h2>Quản lý khoá học</h2>
                <DataTableStyleWrap>
                    <div class="ninjadash-datatable-filter">
                        <div class="ninjadash-datatable-filter__left left-panel">
                            <div class="ninjadash-datatable-filter__input">

                            </div>
                        </div>
                        <div class="ninjadash-datatable-filter__right right-panel">
                            <a-input @change="handleDataUser" size="default" placeholder="Tìm kiếm" class="search-input">
                                <template #prefix>
                                    <unicon name="search"></unicon>
                                </template>
                            </a-input>
                            <div class="ninjadash-datatable-filter__action">
                                <sdButton type="primary" size="lg" @click="showModalCreate">Thêm mới</sdButton>
                            </div>
                        </div>
                        <ModalCreateKhoaHoc :type="modalCreateState.type" :visible="modalCreateState.visible"
                            :onOk="handleModalCreateOk" :onCancel="handleModalCreateCancel"
                            :formState="modalCreateFormState" />
                    </div>
                    <TableWrapper>
                        <a-table :columns="tableHeader" :data-source="displayedData" :pagination="false"
                            :scroll="{ x: 500 }" :responsive="true">
                            <template #headerCell="{ title }"><span :class="`table-head-title`">{{ title
                            }}</span></template>
                            <template #bodyCell="{ column, record }">
                                <template v-if="column.key === 'tenMonHoc'">
                                    <div class="table-body-item courses-name">
                                        <Slider :id="record.id" :arrayData="record.tenMonHoc"
                                            :handlePrevClick="() => slide(record.id, -1)"
                                            :handleNextClick="() => slide(record.id, 1)" :key="record.id" />
                                    </div>
                                </template>
                                <template v-else-if="column.key === 'thaoTac'">
                                    <div class="table-body-item course-operation">
                                        <div class="course-operation-icon course-operation-edit" 
                                            :id="`edit-course-${record.id}`" >
                                            <unicon name="edit"></unicon>
                                        </div>
                                        <div class="course-operation-icon course-operation-delete"
                                            :id="`delete-course-${record.id}`" @click="()=>showModalDelete(record.id)">
                                            <unicon name="trash-alt"></unicon>
                                        </div>
                                    </div>
                                </template>
                                <template v-else>
                                    <div
                                        :class="`table-body-item table-course-${column.key} ${column.key === 'tenKhoa' ? 'truncate-text' : ''}`">
                                        {{ record[column.dataIndex] }}
                                    </div>
                                </template>
                            </template>
                        </a-table>
                        <div class="pagination-wrapper">
                            <a-pagination v-model="currentPage" :total="totalItem" :pageSize="pageSize" show-size-changer
                                @show-size-change="handleSizeChange" @change="handlePageChange" />
                        </div>
                    </TableWrapper>
                </DataTableStyleWrap>
                <ModalDelete :visible="modalDeleteState.visible" :type="modalDeleteState.type" :confirmDelete="()=>confirmDelete(modalDeleteState.currentId)" :handleCancel="handleModalDeleteCancel"/> 

            </div>
        </Main>
    </div>
</template>
<script setup lang="ts">
//import
import { ref, computed, onMounted, reactive } from 'vue';
import Slider from "@/views/pages/Slider.vue"
import { TableWrapper } from "@/views/styled"
import { DataTableStyleWrap } from "@/components/table/Style";
import { Main } from '@/views/styled';
import { BreadcrumbWrapperStyle } from "@/views/uiElements/ui-elements-styled";
import ModalCreateKhoaHoc from "@/views/pages/ModalCreateKhoaHoc.vue"
import ModalDelete from './ModalDelete.vue';

const pageSize = ref(5);
const currentPage = ref(1);
let confirmLoading = ref(false);
const modalCreateState = reactive({
    visible: false,
    type: 'primary',
})
const modalCreateFormState = reactive({
    center: 'center1',
    courseName: '',
    courseSymbol: '',
    active: false,
    shortDesc: '',
    avatar: [],
    illustration: [],
})
const modalDeleteState = reactive({
    visible: false,
    type: 'primary',
    currentId: -1,
})

const tableHeader = [
    {
        title: "KH",
        dataIndex: "kyHieu",
        key: "kyHieu",
    },
    {
        title: "Tên khoá",
        dataIndex: "tenKhoa",
        key: "tenKhoa",
    },
    {
        title: "Lộ trình",
        dataIndex: "tenMonHoc",
        key: "tenMonHoc",
    },
    {
        title: "Thao tác",
        key: "thaoTac",
    },
]
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
        id: 7,
        trungTamId: 1,
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
        id: 12,
        trungTamId: 1,
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
        id: 14,
        trungTamId: 1,
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
        id: 15,
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
        id: 17,
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
        id: 18,
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
        id: 19,
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
        id: 20,
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
const totalItem = computed(() => {
    return courseData.length;
})
const displayedData = computed(() => {
    const start = (currentPage.value - 1) * pageSize.value;
    const end = currentPage.value * pageSize.value;
    return courseData.slice(start, end);
})
onMounted(() => {
})


const handlePageChange = (page: any) => {
    currentPage.value = page;
}
const handleDataUser = () => {

}
const showModalCreate = () => {
    modalCreateState.visible = true;
}
const showModalDelete = (id: number) => {
    modalDeleteState.currentId = id;
    modalDeleteState.visible = true;
}
const handleSizeChange = () => {

}

const handleModalCreateOk = () => {
    confirmLoading.value = true;
    setTimeout(() => {
        modalCreateState.visible = false;
        confirmLoading.value = false;
    }, 2000);
}
const confirmDelete = (id: number) => {
    console.log("Đã xoá thành công " + id);
}
const handleModalCreateCancel = () => {
    modalCreateState.visible = false;
}
const handleModalDeleteCancel = () => {
    modalDeleteState.visible = false;
    modalDeleteState.currentId = -1;
}
const slide = (id: number, direction: any) => {
    const container = document.querySelector(`.slide-group-${id}`);
    if (!container) return null;
    const step = 250;
    const currentScrollLeft = container.scrollLeft;
    const newScrollLeft = currentScrollLeft + direction * step;
    (container as HTMLElement).style.scrollBehavior = 'smooth';
    container.scrollLeft = newScrollLeft;
    setTimeout(() => {
        (container as HTMLElement).style.scrollBehavior = 'auto';
    }, 500);
}

</script>
<style scoped>
:global(.ant-breadcrumb .ant-breadcrumb-link a) {
    color: black !important;
}

:global(.ant-breadcrumb .ant-breadcrumb-link .router-link-active) {
    color: #8231D3 !important;
}

:global(.ant-pagination-disabled button) {
    cursor: default !important;
}

:global(.ant-pagination-prev.ant-pagination-disabled button svg) {
    fill: #9299b8 !important;
}

:global(.ant-pagination-next.ant-pagination-disabled button svg) {
    fill: #9299b8 !important;
}

:global(.ant-pagination-prev button svg) {
    fill: black !important;
}

:global(.ant-pagination-next button svg) {
    fill: black !important;
}

.course-container {
    padding: 16px 30px 0;
}

.lmeiNC {
    display: flex;
    flex-direction: column;
}

.right-panel {
    display: flex;
}

.search-input {
    width: 30rem;
    margin-right: 10px;
}

div.course-content>div>div.fFruaH {
    height: 32.5rem;
    display: flex;
    flex-direction: column;
}

main>div>div>div.course-content>div {
    position: relative;
}

:global(div.pagination-wrapper > ul > li.ant-pagination-options) {
    position: absolute;
    top: 0;
    left: 0;
}

.ant-table-wrapper {
    flex-grow: 1;
}

.table-head-title {
    display: block;
    width: 100%;
    text-align: center;
}

.table-body-item {
    width: 100%;
    text-align: center;
}

.table-course-kyHieu,
.ant-table-cell[colstart="0"] .table-head-title {
    width: 3.125rem;
}

.table-course-tenKhoa,
.ant-table-cell[colstart="1"] .table-head-title {
    width: 15rem;
}

.table-course-thaoTac,
.ant-table-cell[colstart="3"] .table-head-title {
    width: 4rem;
}

:global(.ant-table-cell[colstart="3"]) {
    display: flex;
    justify-content: center;
}

.ant-table-cell[colstart="3"] .table-head-title {
    display: block;
}

.course-content {
    flex-grow: 1;
    padding: 12px 24px 24px;
    margin-top: 12px;
    background-color: white;
    border-radius: 12px;
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
}

.course-operation {
    display: flex;
    justify-content: space-evenly;
}

.course-operation-icon {
    padding: 4px;
    cursor: pointer;
}

:global(.course-operation-edit > div > svg) {
    fill: #8231d3 !important;
}

:global(.course-operation-delete > div > svg) {
    fill: rgb(247, 92, 92) !important;
}

.pagination-wrapper {
    display: flex;
    justify-content: center;
}
</style>