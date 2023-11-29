<template>
    <div class="tour-info-modal">
        <sdModal :type="type" :visible="visible" :onOk="onOk" :onCancel="onCancel" width="45rem"
            :bodyStyle="{ overflowY: 'auto', maxHeight: '92vh' }">
            <div class="tournament-content">
                <div class="tournament-banner">
                    <img :src="modalInfoState.banner"
                        alt="Ảnh post">
                </div>
                <div class="tournament-info">
                    <h3 class="tournament-title">
                        {{ modalInfoState.title }}
                    </h3>
                    <div class="tournament-attended">
                        <span style="margin-right: 16px;">
                            Số người: {{modalInfoState.attendeesNumber}}/{{ modalInfoState.attendeesLimit }}
                        </span>
                        <sdButton v-if="modalInfoState.rate" type="primary" shape="round" size="sm">Rate</sdButton>
                    </div>
                </div>
            </div>
            <div class="tournament-user">
                <div>
                    <img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/users/tsreaper/avatar_1627139236.png"
                        alt="Poster" class="tournament-user-avatar" />
                </div>
                <div class="tournament-user-detail">
                    <h3 class="tournament-user-name">Ngô Quốc Việt</h3>
                    <sdButton type="primary" shape="round" size="sm">Rank: Bạch kim</sdButton>
                </div>
            </div>
            <div class="tournament-detail">
                <a-row>
                    <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="12" :xs="24" class="tournament-detail-item">
                        Tổng số vòng đấu: {{ modalInfoState.roundTotal }}
                    </a-col>
                    <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="12" :xs="24" class="tournament-detail-item">
                        Vòng đấu hiện tại: {{ modalInfoState.currentRound }}
                    </a-col>
                </a-row>
                <a-row>
                    <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="12" :xs="24" class="tournament-detail-item">
                        Ngày bắt đầu: 26/11/2023
                    </a-col>
                    <a-col :xxl="12" :xl="12" :lg="12" :md="12" :sm="12" :xs="24" class="tournament-detail-item">
                        Ngày kêt thúc: 26/12/2023
                    </a-col>
                </a-row>
            </div>
            <div class="tournament-desc-container">
                <div class="tournament-desc-title">
                    Mô tả
                </div>
                <div class="tournament-desc-detail">
                    {{ modalInfoState.shortDescription }}
                </div>
            </div>
            <div class="tournament-desc-action">
                <sdButton v-if="modalInfoState.status!=='upcoming'" type="light" shape="round" size="lg">Xem danh sách</sdButton>
                <sdButton v-if="modalInfoState.status==='upcoming'" type="primary" shape="round" size="lg">Đăng ký tham gia</sdButton>
            </div>
        </sdModal>
    </div>
</template>

<script setup lang="ts">
const props = defineProps([
    'visible',
    'onOk',
    'onCancel',
    'type',
    'tourInfo',
    'modalInfoState'
]);
</script>
<style scoped>
:global(body > div:nth-child(11) > div > div.ant-modal-wrap) {
    display: flex;
    align-items: center;
    justify-content: center;
}

:global(body > div:nth-child(11) > div > div.ant-modal-wrap > div) {
    top: 0;
    padding-bottom: 0;
}
:global(body > div:nth-child(10) > div > div.ant-modal-wrap > div > div.ant-modal-content > div > div.tournament-content > div.tournament-info > div > button) {
    pointer-events: none;
}
:global(body > div:nth-child(10) > div > div.ant-modal-wrap > div > div.ant-modal-content > div > div.tournament-user > div.tournament-user-detail > button) {
    pointer-events: none;
}
:global(body > div:nth-child(10) > div > div.ant-modal-wrap > div > div.ant-modal-content > div > div.tournament-content > div.tournament-info > div > button) {
    cursor: default;
}
.tournament-content {
    display: flex;
    margin-bottom: 3.125rem;
}

.tournament-banner img {
    height: 9rem;
    width: 16rem;
    border-radius: 16px;
}

.tournament-info {
    margin-left: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
}

.tournament-title {
    font-size: 1.5rem;
    font-weight: 600;
}

.tournament-attended {
    font-size: 1.125rem;
}

.tournament-user {
    display: flex;
    align-items: center;
    margin-bottom: 1.5rem;
}

.tournament-user-avatar {
    width: 5rem;
    width: 5rem;
    border-radius: 50%;
    margin-right: 1rem;
}

.tournament-user-name {
    font-size: 1.25rem;
    font-weight: 500;
}
.tournament-detail {
    margin-bottom: 1.5rem;
}

.tournament-detail .tournament-detail-item {
    font-size: 1rem;
    font-weight: 500;
}
.tournament-desc-container {
    background-color: #F9F9F9;
    border-radius: 10px;
    min-height: 8rem;
    padding: 0.5rem;
    margin-bottom: 1.5rem;
}
.tournament-desc-title {
    font-size: 1.2rem;
    font-weight: 500;
    margin-bottom: 0.2rem;
}
.tournament-desc-detail {
    font-size: 1rem;
    font-weight: 400;
}
.tournament-desc-action {
    display: flex;
    justify-content: end;
    gap: 0.55rem;
}
</style>