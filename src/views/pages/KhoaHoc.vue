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
                        <div class="ninjadash-datatable-filter__left left-panel"></div>
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
                            :onOk="createKhoaHoc" :onCancel="handleModalCreateCancel" :formState="modalCreateFormState" />
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
                                            :id="`edit-course-${record.id}`" @click="() => showModalEdit(record.id)">
                                            <unicon name="edit"></unicon>
                                        </div>
                                        <div class="course-operation-icon course-operation-delete"
                                            :id="`delete-course-${record.id}`" @click="() => showModalDelete(record.id)">
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
                        <div class="pagination-wrapper pagination-wrapper-course">
                            <a-pagination v-model="currentPage" :total="totalItem" :pageSize="pageSizeCourse" show-size-changer
                                show-less-items @show-size-change="handleSizeChange" @change="handlePageChange" />
                        </div>
                    </TableWrapper>
                </DataTableStyleWrap>
                <ModalDelete :visible="modalDeleteState.visible" :type="modalDeleteState.type"
                    :confirmDelete="() => deleteKhoaHoc(modalDeleteState.currentId)"
                    :handleCancel="handleModalDeleteCancel" />
                <ModalEditKhoaHoc :visible="modalEditState.visible" :type="modalEditState.type"
                    :onOk="() => updateKhoaHoc(modalEditState.currentId)" :onCancel="handleModalEditCancel"
                    :formState="modalEditFormState" :courseSubjectTotal="getSubjectTotal(modalEditState.currentId)"
                    :courseSubjects="getSubjectTableData(modalEditState.currentId)" :courseId="modalEditState.currentId"
                    :deleteSubjectInCourse="deleteSubjectInCourse" />
            </div>
        </Main>
    </div>
</template>
<script setup lang="ts">
//import
import { ref, computed, onMounted, reactive, watchEffect } from 'vue';
import Slider from "@/views/pages/Slider.vue";
import { TableWrapper } from "@/views/styled";
import { DataTableStyleWrap } from "@/components/table/Style";
import { Main } from '@/views/styled';
import { BreadcrumbWrapperStyle } from "@/views/uiElements/ui-elements-styled";
import ModalCreateKhoaHoc from "@/views/pages/ModalCreateKhoaHoc.vue";
import ModalEditKhoaHoc from "@/views/pages/ModalEditKhoaHoc.vue";
import ModalDelete from './ModalDelete.vue';
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
const courseImageData = [
    {
        id: 1,
        avatarUrl: "https://fsoft-academy.edu.vn/wp-content/uploads/2021/08/FS.NET_-1024x540.png",
        illustrationUrl: "https://www.edureka.co/blog/wp-content/uploads/2018/11/Full-Stack-Developer-RoadMap-How-To-Become-A-Full-Stack-Developer-Edureka.png",
    },
    {
        id: 2,
        avatarUrl: "https://fsoft-academy.edu.vn/wp-content/uploads/2021/08/FS.Java_-1024x540.png",
        illustrationUrl: "https://www.edureka.co/blog/wp-content/uploads/2018/11/Full-Stack-Developer-RoadMap-How-To-Become-A-Full-Stack-Developer-Edureka.png",
    },
    {
        id: 3,
        avatarUrl: "https://d3hi6wehcrq5by.cloudfront.net/itnavi-blog/2020/04/Tester-l%C3%A0-g%C3%AC-4.jpg",
        illustrationUrl: "https://www.edureka.co/blog/wp-content/uploads/2018/11/Full-Stack-Developer-RoadMap-How-To-Become-A-Full-Stack-Developer-Edureka.png",
    },
    {
        id: 4,
        avatarUrl: "https://softwaretester.careers/wp-content/uploads/2018/08/istqb-foundation-exam.png",
        illustrationUrl: "https://www.edureka.co/blog/wp-content/uploads/2018/11/Full-Stack-Developer-RoadMap-How-To-Become-A-Full-Stack-Developer-Edureka.png",
    },
    {
        id: 5,
        avatarUrl: "https://www.softwaretestinghelp.com/wp-content/qa/uploads/2020/04/ISTQB-Test-Manager-Certification-Series.png",
        illustrationUrl: "https://www.edureka.co/blog/wp-content/uploads/2018/11/Full-Stack-Developer-RoadMap-How-To-Become-A-Full-Stack-Developer-Edureka.png",
    },
    {
        id: 6,
        avatarUrl: "https://th.bing.com/th/id/OIP.RyCn3j60F_txTiYhRgLxjQHaDO?w=1024&h=447&rs=1&pid=ImgDetMain",
        illustrationUrl: "https://www.edureka.co/blog/wp-content/uploads/2018/11/Full-Stack-Developer-RoadMap-How-To-Become-A-Full-Stack-Developer-Edureka.png",
    },
    {
        id: 7,
        avatarUrl: "https://28tech.com.vn/assets/img/khoa-hoc/c-plus-plus.png",
        illustrationUrl: "https://www.edureka.co/blog/wp-content/uploads/2018/11/Full-Stack-Developer-RoadMap-How-To-Become-A-Full-Stack-Developer-Edureka.png",
    },
    {
        id: 8,
        avatarUrl: "https://media.geeksforgeeks.org/wp-content/cdn-uploads/20220416200936/Top-10-Front-End-Developer-Skills-That-You-Need-in-2022.png",
        illustrationUrl: "https://www.edureka.co/blog/wp-content/uploads/2018/11/Full-Stack-Developer-RoadMap-How-To-Become-A-Full-Stack-Developer-Edureka.png",
    },
    {
        id: 9,
        avatarUrl: "https://i.ytimg.com/vi/Q2YhTNS-VkA/maxresdefault.jpg",
        illustrationUrl: "https://www.edureka.co/blog/wp-content/uploads/2018/11/Full-Stack-Developer-RoadMap-How-To-Become-A-Full-Stack-Developer-Edureka.png",
    },
];
const centerData = [
    {
        id: 1,
        tenTrungTam: 'Lotus Academy',
        value: 'center1',
    },
    {
        id: 2,
        tenTrungTam: 'Đặng Tiến Đông',
        value: 'center2',
    },
    {
        id: 3,
        tenTrungTam: 'Mỹ Đình',
        value: 'center3'
    }
]
const courseSubjectData = courseData.map(course => {
    return {
        id: course.id,
        data: course.tenMonHoc.map((subject, index) => ({
            id: index + 1,
            order: index + 1,
            name: subject,
        })),
    };
});

const totalItem = computed(() => {
    return courseData.length;
})
const displayedData = computed(() => {
    const start = (currentPage.value - 1) * pageSizeCourse.value;
    const end = currentPage.value * pageSizeCourse.value;
    return courseData.slice(start, end);
})

const pageSizeCourse = ref(5);
const currentPage = ref(1);
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
    apiUpload: '/api/upload'
})
const modalDeleteState = reactive({
    visible: false,
    type: 'primary',
    currentId: -1,
})
const modalEditState = reactive({
    visible: false,
    type: 'primary',
    currentId: -1,
})
const modalEditFormState = reactive({
    center: ref(''),
    avatarPreview: ref(''),
    illustrationPreview: ref(''),
    courseName: ref(''),
    courseSymbol: ref(''),
    active: ref(false),
    shortDesc: ref(''),
    avatar: ref([]),
    illustration: ref([]),
    apiUpload: ref('/api/upload'),
    createSubjectName: ref(''),
    createSubjectOrder: ref(''),
});
watchEffect(() => {
    const centerId = courseData.find(item => item.id === modalEditState.currentId)?.trungTamId;
    const center = centerData.find(item => item.id === centerId)?.tenTrungTam;
    modalEditFormState.center = center !== undefined ? center : '';

    const avatarPreview = courseImageData.find(item => item.id === modalEditState.currentId)?.avatarUrl;
    modalEditFormState.avatarPreview = avatarPreview !== undefined ? avatarPreview : '';

    const illustrationPreview = courseImageData.find(item => item.id === modalEditState.currentId)?.illustrationUrl;
    modalEditFormState.illustrationPreview = illustrationPreview !== undefined ? illustrationPreview : '';

    const courseName = courseData.find(item => item.id === modalEditState.currentId)?.tenKhoa;
    modalEditFormState.courseName = courseName !== undefined ? courseName : '';

    const courseSymbol = courseData.find(item => item.id === modalEditState.currentId)?.kyHieu;
    modalEditFormState.courseSymbol = courseSymbol !== undefined ? courseSymbol : '';
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
const showModalEdit = (id: number) => {
    modalEditState.currentId = id;
    modalEditState.visible = true;
}
const getSubjectTableData = (id: number) => {
    const tableData = courseSubjectData.find(course => course.id == id)?.data;
    return tableData;
}
const getSubjectTotal = (id: number) => {
    const tableData = courseSubjectData.find(course => course.id == id)?.data;
    const length = tableData?.length;
    return length;
}
const deleteSubjectInCourse = (courseId: number, subjectId: number) => {
    const tableData = courseSubjectData.find(course => course.id == courseId)?.data;
    const subject = tableData?.find(subject => subject.id == subjectId);
    // console.log(subject);
    console.log("Xoá thành công môn học " + subjectId + " của khoá " + courseId);
}
const handleSizeChange = () => {

}

const createKhoaHoc = () => {
    const createData = {
        courseName: modalCreateFormState.courseName,
        courseSymbol: modalCreateFormState.courseSymbol,
        center: modalCreateFormState.center,
        active: modalCreateFormState.active,
        shortDesc: modalCreateFormState.shortDesc,
        avatar: modalCreateFormState.avatar,
        illustration: modalCreateFormState.illustration,
    }
    console.log(createData);
    // setTimeout(() => {
    //     modalCreateState.visible = false;
    // }, 1000);
}
const updateKhoaHoc = (id: number) => {
    const updateData = {
        courseName: modalEditFormState.courseName,
        courseSymbol: modalEditFormState.courseSymbol,
        center: modalEditFormState.center,
        active: modalEditFormState.active,
        shortDesc: modalEditFormState.shortDesc,
        avatar: modalEditFormState.avatar,
        illustration: modalEditFormState.illustration,
        createSubjectName: modalEditFormState.createSubjectName,
        createSubjectOrder: modalEditFormState.createSubjectOrder
    };
    console.log(updateData);
    // setTimeout(() => {
    //     modalEditState.visible = false;
    // }, 1000);
}
const deleteKhoaHoc = (id: number) => {
    console.log("Xoá thành công khoá học" + id);
    // setTimeout(() => {
    //     modalDeleteState.visible = false;
    // }, 1000);
}

const handleModalCreateCancel = () => {
    modalCreateState.visible = false;
}
const handleModalDeleteCancel = () => {
    modalDeleteState.visible = false;
    modalDeleteState.currentId = -1;
}
const handleModalEditCancel = () => {
    modalEditState.visible = false;
    modalEditState.currentId = -1;
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

:global(div.pagination-wrapper-course > ul > li.ant-pagination-options) {
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