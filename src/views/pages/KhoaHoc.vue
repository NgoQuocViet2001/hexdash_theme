<template>
    <div class="course-container">
        <Main>
            <BreadcrumbWrapperStyle>
                <a-breadcrumb>
                    <a-breadcrumb-item>
                        <router-link to="/" >Home</router-link>
                    </a-breadcrumb-item>
                    <a-breadcrumb-item>
                        <router-link to="/noidung/khoahoc">Khoá học</router-link>
                    </a-breadcrumb-item>
                </a-breadcrumb>
            </BreadcrumbWrapperStyle>
            <div class="course-content">
                <h2>Quản lý khoá học</h2>
            </div>
        </Main>
    </div>
</template>
<script setup lang="ts">
//import
import { Main } from '@/views/styled';
import { BreadcrumbWrapperStyle } from "@/views/uiElements/ui-elements-styled";

</script>
<style scoped>
.course-container {
    padding: 16px 30px 0;
}
.lmeiNC {
    display: flex;
    flex-direction: column;
}
.course-content {
    flex-grow: 1;
    padding: 12px 24px 24px;
    margin-top: 12px;
    background-color: white;
    border-radius: 12px;
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
}
</style>