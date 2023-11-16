
<template>
    <div class="challenge-container">
        <Main>
            <!-- Phần breadcrumb -->
            <BreadcrumbWrapperStyle>
                <a-breadcrumb>
                    <a-breadcrumb-item>
                        <RouterLink to="/">Home</RouterLink>
                    </a-breadcrumb-item>
                    <a-breadcrumb-item>
                        <RouterLink to="/challenge">Thử thách</RouterLink>
                    </a-breadcrumb-item>
                </a-breadcrumb>
            </BreadcrumbWrapperStyle>
            <!-- Phần trên: Tạo thử thách -->
            <div class="create-challenge">
                <h2>Bắt đầu thử thách</h2>
                <a-row :gutter=[16,8]>
                    <a-col :xxl="8" :xl="8" :xs="24" v-for=" item  in  challengeItems " :key="item.title">
                        <ChallengeCard :backgroundImage="item.backgroundImage" :title="item.title" :desc="item.desc"
                            :btnText="item.btnText" :btnClick="() => showModal(item.id)" />
                    </a-col>
                    <CreateChallengeModal :type="modalType" :visible="modalState.visible" :onOk="handleOk" :onCancel="handleCancel"
                    :modalID="modalState.currentModalID" :formState="modalState.formState" />
                </a-row>
            </div>
    
            <!-- Phần giữa: Thử thách đang diễn ra và ranking -->
            <div class="challenge-info">
                <a-row :gutter=[16,8]>
                    <a-col :xxl="18" :xl="24" :xs="24">
                        <div class="challenge-inprogress">
                            <h2>Thử thách đang diễn ra</h2>
                            <TabBasic v-model:activeKey="activeKey">
                                <Child v-for="(tab, index) in tabs" :key="index + 1">
                                    <template #tab><span class="tab-label">{{ tab.label }}</span></template>
                                    <div class="select-status">
                        
                                            <a-select v-if="index == 0" v-model:value="selectState" style="width: 100%" @change="handleChangeStatus">
                                                <a-select-option value="happening">Đang diễn ra</a-select-option>
                                                <a-select-option value="upcoming">Sắp diễn ra</a-select-option>
                                                <a-select-option value="ended">Đã diễn ra</a-select-option>
                                            </a-select>
    
                                    </div>
                                    <div v-if="activeKey === tab.id" class="tab-content" >
                                        <a-row :gutter=[16,24] v-if="tab.id === 1 || tab.id === 2" :class= "`tab-list tab-list-${selectState}`">
                                            <a-col :xxl="12" :xl="12" :xs="24"
                                                v-for="(item, index) in tab.id === 1 ? displayedTournamentItems : displayedSoloItems"
                                                :key="index">
                                                <div class="challenge-inprogress-card">
                                                    <div class="challenge-inprogress-info">
                                                        <img :src="item.poster" alt="Poster" class="challenge-inprogress-img" />
                                                        <div class="challenge-inprogress-detail">
                                                            <h3>{{ item.title }}</h3>
                                                            <p>{{ item.startTime }}</p>
                                                        </div>
                                                    </div>
                                                    <span class="challenge-inprogress-status">{{ item.status }}</span>
                                                </div>
    
                                            </a-col>
                                        </a-row>
                                    </div>
                                    <div class="pagination-wrapper">
                                        <sdCards v-if="tab.id == 1 && selectState == 'happening'" class="challenge-pagination">
                                            <a-pagination v-model="happeningTourCurrentPage" :total="tournamentTotalItems"
                                                :show-size-changer="false" :pageSize="tournamentPageSize" show-less-items
                                                @change="handleHappeningPage" />
                                        </sdCards>
                                        <sdCards v-if="tab.id == 1 && selectState == 'upcoming'" class="challenge-pagination">
                                            <a-pagination v-model="upcomingTourCurrentPage" :total="tournamentTotalItems"
                                                :show-size-changer="false" :pageSize="tournamentPageSize" show-less-items
                                                @change="handleUpcomingPage" />
                                        </sdCards>
                                        <sdCards v-if="tab.id == 1 && selectState == 'ended'" class="challenge-pagination">
                                            <a-pagination v-model="endedTourCurrentPage" :total="tournamentTotalItems"
                                                :show-size-changer="false" :pageSize="tournamentPageSize" show-less-items
                                                @change="handleEndedChange" />
                                        </sdCards>
                                        <sdCards v-if="tab.id == 2" class="challenge-pagination">
                                            <a-pagination v-model="soloCurrentPage" :total="soloTotalItems"
                                                :show-size-changer="false" :pageSize="soloPageSize" show-less-items
                                                @change="handleSoloPageChange" />
                                        </sdCards>
                                    </div>
                                </Child>
                            </TabBasic>
                        </div>
                    </a-col>
                    <a-col :xxl="6" :xl="24" :xs="24">
                        <div class="ranking-top">
                            <div class="ranking-title">
                                <h2 style="margin-bottom: 8px;">Bảng xếp hạng</h2>
                            </div>
                            <a-list :item-layout="'horizontal'" :dataSource="selectTopRankingUsers">
                                <template #renderItem="{ item }">
                                    <div class="ranking-user" :style="{ 'border-color': getRankingStyle(item.order).color }">
                                        <div class="ranking-user-avatar">
                                            <img :src="item.avatar" alt="Avatar" />
                                        </div>
                                        <div class="ranking-user-meta">
                                            <h3 class="ranking-user-title"
                                                :style="{ color: getRankingStyle(item.order).color }">
                                                <img v-if="getRankingStyle(item.order).icon"
                                                    :src="getRankingStyle(item.order).icon" alt="Ranking Icon"
                                                    class="ranking-medal" />
                                                <span v-else>{{ `${item.order}.` }}</span>
                                                {{ item.name }}
                                            </h3>
                                            <p class="ranking-user-description">{{ `Elo: ${item.elo} | Attended:
                                                                                        ${item.attended}` }}</p>
                                        </div>
                                    </div>
                                </template>
                            </a-list>
                            <Buttons class="btn-ranking-view" type="primary" @click="toggleViewMore">{{ viewMore ? 'Ẩn bớt' :
        'Xem thêm' }}</Buttons>
                        </div>
                    </a-col>
                </a-row>
            </div>
        </Main>
    </div>
</template>
  
<script setup lang="ts">
//import
import { Main } from '@/views/styled';
import { ChallengeCard } from "@/components/banners/Banners.vue";
import { BreadcrumbWrapperStyle } from "@/views/uiElements/ui-elements-styled";
import Buttons from "@/components/buttons/Buttons.vue"
import CreateChallengeModal from "./CreateChallengeModal.vue";
import { TabBasic, Child } from "@/components/tabs/Style";
import { ref, computed, onMounted, reactive } from 'vue';

// DATA
//formstate
const modalState = reactive({
    currentModalID: 0,
    formState: computed(() => {
        switch (modalState.currentModalID) {
            case 1:
                return tournamentFormState;
            case 2:
                return soloFormState;
            case 3:
                return trainFormState;
            default:
                return tournamentFormState;
        }
    }),
    visible: false,
});
const tournamentFormState = reactive({
    description: '',
    start: '',
    end: '',
    level: '',
    challengeTime: '',
});
const soloFormState = reactive({
    description: '',
    start: '',
    end: '',
    level: '',
    challengeTime: '',
});
const trainFormState = reactive({
    description: '',
    start: '',
    end: '',
    level: '',
    challengeTime: '',
});


// Dữ liệu mẫu cho phần Tạo thử thách
const challengeItems = [
    {
        id: 1,
        title: "Giải đấu",
        desc: "Tạo giải đấu nhiều người với nhiều phần thưởng hấp dẫn",
        backgroundImage: "https://assets.leetcode.com/contest/weekly-contest-291/card_img_1654267951.png",
        btnText: "Tạo"
    },
    {
        id: 2,
        title: "So tài",
        desc: "So tài code với bạn bè và người khác",
        backgroundImage: "https://assets.leetcode.com/contest/weekly-contest-290/card_img_1654267980.png",
        btnText: "Tạo"
    },
    {
        id: 3,
        title: "Luyện tập",
        desc: "Luyện tập code không giới hạn",
        backgroundImage: "https://leetcode.com/_next/static/images/weekly-default-553ede7bcc8e1b4a44c28a9e4a32068c.png",
        btnText: "Tạo"
    },
];
// tabs
const tabs = [
    {
        id: 1,
        label: "Giải đấu",
    },
    {
        id: 2,
        label: "So tài",
    },
];

// Dữ liệu mẫu cho phần Thử thách đang diễn ra
const challengeInProgress = {
    tournament: [
        {
            title: "Đấu Đỉnh Cao",
            poster: "https://www.educative.io/v2api/editorpage/4994081693368320/image/5898426517553152",
            startTime: "2023-11-09 10:00 AM",
            status: "happening",
        },
        {
            title: "Thách Thức Javascript",
            poster: "https://assets.leetcode.com/static_assets/others/JS_30_-_240x240.png",
            startTime: "2023-11-09 10:00 AM",
            status: "happening",
        },
        {
            title: "Cuộc Chiến Amazon",
            poster: "https://assets.leetcode.com/static_assets/others/Amazon_Spring_23_High_Frequency1.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "CodeMasters Challenge",
            poster: "https://leetcode.com/_next/static/images/biweekly-default-f5a8fc3be85b6c9175207fd8fd855d47.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Giải Đấu SQL",
            poster: "https://assets.leetcode.com/static_assets/others/Top_SQL_50_static_cover_picture.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Giải Đấu Dynamic Programming",
            poster: "https://assets.leetcode.com/contest/biweekly-contest-85/card_img_1659801683.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Kĩ thuật thực nghiệm code",
            poster: "https://assets.leetcode.com/static_assets/others/Dynamic_Programming_static_cover_picture.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Thuật Toán Vô Song",
            poster: "https://assets.leetcode.com/study_plan_v2/programming-skills/cover",
            startTime: "2023-11-09 10:00 AM",
            status: "ended",
        },
        {
            title: "Thách Thức HackerRank",
            poster: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTnuVnHrtRouWTFgSkjp49VZal0bT3crXyWrA&usqp=CAU",
            startTime: "2023-11-09 10:00 AM",
            status: "ended",
        },
        {
            title: "Đại Chiến Strings",
            poster: "https://cdn-blog.28tech.com.vn/media/c%2B%2B%20tutorial/string/string%20trong%20cpp.png",
            startTime: "2023-11-09 10:00 AM",
            status: "happening",
        },
        {
            title: "Lập trình viên siêu cấp",
            poster: "https://yt3.googleusercontent.com/A0i8xdbzNoYrO6dGSoS3eBPUAnhKmU9MDXtS0xI2kD6buLEi8klyPfLpIkmUkhOiZ4mEAj6wSQ=s900-c-k-c0x00ffffff-no-rj",
            startTime: "2023-11-09 10:00 AM",
            status: "happening",
        },
        {
            title: "Lập Trình Nghệ Thuật",
            poster: "https://repository-images.githubusercontent.com/72586805/05dd0b80-6b7c-11e9-9cf5-15d6d7efb700",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Nghệ Sĩ Code Đỉnh Cao",
            poster: "https://assets.leetcode.com/contest/weekly-contest-291/card_img_1654267951.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Đấu Đỉnh Cao",
            poster: "https://www.educative.io/v2api/editorpage/4994081693368320/image/5898426517553152",
            startTime: "2023-11-09 10:00 AM",
            status: "happening",
        },
        {
            title: "Thách Thức Javascript",
            poster: "https://assets.leetcode.com/static_assets/others/JS_30_-_240x240.png",
            startTime: "2023-11-09 10:00 AM",
            status: "happening",
        },
        {
            title: "Cuộc Chiến Amazon",
            poster: "https://assets.leetcode.com/static_assets/others/Amazon_Spring_23_High_Frequency1.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "CodeMasters Challenge",
            poster: "https://leetcode.com/_next/static/images/biweekly-default-f5a8fc3be85b6c9175207fd8fd855d47.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Giải Đấu SQL",
            poster: "https://assets.leetcode.com/static_assets/others/Top_SQL_50_static_cover_picture.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Giải Đấu Dynamic Programming",
            poster: "https://assets.leetcode.com/contest/biweekly-contest-85/card_img_1659801683.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Kĩ thuật thực nghiệm code",
            poster: "https://assets.leetcode.com/static_assets/others/Dynamic_Programming_static_cover_picture.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Thuật Toán Vô Song",
            poster: "https://assets.leetcode.com/study_plan_v2/programming-skills/cover",
            startTime: "2023-11-09 10:00 AM",
            status: "ended",
        },
        {
            title: "Thách Thức HackerRank",
            poster: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTnuVnHrtRouWTFgSkjp49VZal0bT3crXyWrA&usqp=CAU",
            startTime: "2023-11-09 10:00 AM",
            status: "ended",
        },
        {
            title: "Đại Chiến Strings",
            poster: "https://cdn-blog.28tech.com.vn/media/c%2B%2B%20tutorial/string/string%20trong%20cpp.png",
            startTime: "2023-11-09 10:00 AM",
            status: "happening",
        },
        {
            title: "Lập trình viên siêu cấp",
            poster: "https://yt3.googleusercontent.com/A0i8xdbzNoYrO6dGSoS3eBPUAnhKmU9MDXtS0xI2kD6buLEi8klyPfLpIkmUkhOiZ4mEAj6wSQ=s900-c-k-c0x00ffffff-no-rj",
            startTime: "2023-11-09 10:00 AM",
            status: "happening",
        },
        {
            title: "Lập Trình Nghệ Thuật",
            poster: "https://repository-images.githubusercontent.com/72586805/05dd0b80-6b7c-11e9-9cf5-15d6d7efb700",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Nghệ Sĩ Code Đỉnh Cao",
            poster: "https://assets.leetcode.com/contest/weekly-contest-291/card_img_1654267951.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "Đấu Đỉnh Cao",
            poster: "https://www.educative.io/v2api/editorpage/4994081693368320/image/5898426517553152",
            startTime: "2023-11-09 10:00 AM",
            status: "happening",
        },
        {
            title: "Thách Thức Javascript",
            poster: "https://assets.leetcode.com/static_assets/others/JS_30_-_240x240.png",
            startTime: "2023-11-09 10:00 AM",
            status: "happening",
        },
        {
            title: "Cuộc Chiến Amazon",
            poster: "https://assets.leetcode.com/static_assets/others/Amazon_Spring_23_High_Frequency1.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
        {
            title: "CodeMasters Challenge",
            poster: "https://leetcode.com/_next/static/images/biweekly-default-f5a8fc3be85b6c9175207fd8fd855d47.png",
            startTime: "2023-11-09 10:00 AM",
            status: "upcoming",
        },
    ],
    solo: Array.from({ length: 28 }, (_, index) => ({
        title: `So tài ${index + 1}`,
        poster: "https://assets.leetcode.com/contest/weekly-contest-290/card_img_1654267980.png",
        startTime: "2023-11-09 10:00 AM",
        status: "10 phút",
    })),
};
// Dữ liệu mẫu cho top ranking
const topRankingUsers = Array.from({ length: 20 }, (_, index) => ({
    order: index + 1,
    avatar: `https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/users/tsreaper/avatar_1627139236.png`,
    name: `User ${index + 1}`,
    elo: "1500",
    attended: "2000",
}));

//REFS
// const currentModalID = ref(-1);
let modalType = ref('primary');
// let visible = ref(false);
let confirmLoading = ref(false);


let selectState = ref('happening');
let happeningTourCurrentPage = ref(1);
let upcomingTourCurrentPage = ref(1);
let endedTourCurrentPage = ref(1);

let activeKey = ref(1);
// let tournamentCurrentPage = ref(1);
let soloCurrentPage = ref(1);
const tournamentPageSize = ref(12);
const soloPageSize = ref(12);
let viewMore = ref(false);

let happeningTourHeight = ref(0);
let upcomingTourHeight = ref(0);
let endedTourHeight = ref(0);
let soloPageHeight = ref(0);
let rowPage = ref<HTMLElement | null>(null);
// let watched = ref(false);
// MOUNTED
onMounted(() => {
    updateSizeItemImg();
});

//WATCH
// watch(() => activeKey.value, async () => {
//     if (!watched.value) {
//         setSoloPageHeight();
//         watched.value = true;
//     }
// });

//COMPUTED
const tournamentTotalItems = computed(() => {
    const items = challengeInProgress.tournament;
    return items.filter(item => item.status === selectState.value).length;
});
const soloTotalItems = computed(() => {
    const items = challengeInProgress.solo;
    return items.length;
});
const displayedTournamentItems = computed(() => {
    let tournamentCurrentPage = 1;
    if (selectState.value === 'happening') {
        tournamentCurrentPage = happeningTourCurrentPage.value;
    } else if (selectState.value === 'upcoming') {
        tournamentCurrentPage = upcomingTourCurrentPage.value;
    } else if (selectState.value === 'ended') {
        tournamentCurrentPage = endedTourCurrentPage.value;
    }
    const items = challengeInProgress.tournament;
    const startIndex = (tournamentCurrentPage - 1) * soloPageSize.value;
    const endIndex = startIndex + soloPageSize.value;
    return items.filter(item => item.status === selectState.value).slice(startIndex, endIndex);
});


const displayedSoloItems = computed(() => {
    const items = challengeInProgress.solo;
    const startIndex = (soloCurrentPage.value - 1) * soloPageSize.value;
    const endIndex = startIndex + soloPageSize.value;
    return items.slice(startIndex, endIndex);
});
let selectTopRankingUsers = computed(() => {
    return viewMore.value ? topRankingUsers : topRankingUsers.slice(0, 10);
});


//METHODS
function showModal(id: number) {
    modalState.currentModalID = id;
    modalState.visible = true;
}

function handleOk() {
    confirmLoading.value = true;
    setTimeout(() => {
        modalState.visible = false;
        confirmLoading.value = false;
    }, 2000);
}

function handleCancel() {
    modalState.visible = false;
}
const handleChangeStatus = (page: any) => {
    if(selectState.value == 'happening') 
        setHappeningHeight();
    if(selectState.value == 'upcoming') 
        setUpcomingHeight();
    if(selectState.value == 'ended') 
        setEndedHeight();

};
const handleHappeningPage = (page: any) => {
    happeningTourCurrentPage.value = page;
    setHappeningHeight();
};
const handleUpcomingPage = (page: any) => {
    upcomingTourCurrentPage.value = page;
    setUpcomingHeight();
};
const handleEndedChange = (page: any) => {
    endedTourCurrentPage.value = page;
    setEndedHeight();
};
const handleSoloPageChange = (page: any) => {
    soloCurrentPage.value = page;
    setSoloPageHeight();
};
const toggleViewMore = () => {
    viewMore.value = !viewMore.value;
}
const getAspectRatio = () => {
    const screenWidth = window.innerWidth;
    const screenHeight = window.innerHeight;
    return screenWidth / screenHeight;
}
const updateSizeItemImg = () => {
    const screenWidth = window.innerWidth;
    const screenHeight = window.innerHeight;
    const newAspectRatio = getAspectRatio();
    if (newAspectRatio < 0.6) {
        document.documentElement.style.setProperty('--challenge-img-width', `${screenWidth / 3}px`);
        document.documentElement.style.setProperty('--challenge-img-height', `${screenWidth / 2 * newAspectRatio}px`);
    }
    else if (newAspectRatio >= 0.6 && newAspectRatio < 1) {
        document.documentElement.style.setProperty('--challenge-img-width', `${screenWidth * 5 / 12}px`);
        document.documentElement.style.setProperty('--challenge-img-height', `${screenWidth * 5 / 12 * newAspectRatio}px`);
    }
    else {
        document.documentElement.style.setProperty('--challenge-img-height', `${screenHeight / 15 * 2}px`);
        document.documentElement.style.setProperty('--challenge-img-width', `${screenHeight / 15 * 2 * newAspectRatio}px`);
    }
};

const setHappeningHeight = async () => {
    // const childage = document.querySelector(`.tab-list-${selectState}`);
    const page = document.querySelector('.ant-tabs-tabpane-active');
    if (!page || !(page instanceof HTMLElement)) return;
    const pageHeight = page.clientHeight;
        const maxHeight = happeningTourHeight.value;
        if (pageHeight > maxHeight) {
            happeningTourHeight.value = pageHeight;
            rowPage.value = page;
        }
        (page as HTMLElement).style.minHeight = `${happeningTourHeight.value}px`;
};
const setUpcomingHeight = async () => {
    // const page = document.querySelector('.tab-list-${selectState}');
    const page = document.querySelector('.ant-tabs-tabpane-active');
    if (!page || !(page instanceof HTMLElement)) return;
    const pageHeight = page.clientHeight;
        const maxHeight = upcomingTourHeight.value;
        if (pageHeight > maxHeight) {
            upcomingTourHeight.value = pageHeight;
            rowPage.value = page;
        }
        (page as HTMLElement).style.minHeight = `${upcomingTourHeight.value}px`;

};
const setEndedHeight = async () => {
    // const page = document.querySelector('.tab-list-${selectState}');
    const page = document.querySelector('.ant-tabs-tabpane-active');
    if (!page || !(page instanceof HTMLElement)) return;

    const pageHeight = page.clientHeight;
        const maxHeight = endedTourHeight.value;
        if (pageHeight > maxHeight) {
            endedTourHeight.value = pageHeight;
            rowPage.value = page;
        }
        (page as HTMLElement).style.minHeight = `${endedTourHeight.value}px`;

};
const setSoloPageHeight = async () => {
    const page = document.querySelector('.ant-tabs-tabpane-active');
    if (!page || !(page instanceof HTMLElement)) return;

    const pageHeight = page.clientHeight;
    const maxHeight = soloPageHeight.value;
    if (pageHeight > maxHeight) {
        soloPageHeight.value = pageHeight;
        rowPage.value = page;
    }
    (page as HTMLElement).style.height = `${soloPageHeight.value}px`;
};
const getRankingStyle = (order: any) => {
    let color, icon;
    switch (order) {
        case 1:
            color = '#feaa2b';
            icon = 'https://cdn-icons-png.flaticon.com/512/5406/5406792.png';
            break;
        case 2:
            color = '#9e9e9e';
            icon = 'https://cdn-icons-png.flaticon.com/512/2583/2583319.png';
            break;
        case 3:
            color = '#ce7430';
            icon = 'https://cdn-icons-png.flaticon.com/512/2583/2583434.png';
            break;
        default:
            color = '#000000';
    }
    return { color, icon };
};
</script>

<style scoped>
.challenge-container {
    padding-top: 16px;
}

.create-challenge,
.challenge-inprogress,
.ranking-top {
    padding: 12px 24px 24px;
    margin-top: 12px;
    background-color: white;
    border-radius: 12px;
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
}

.ant-tabs-tabpane {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.tab-content {
    flex-grow: 1;
    width: 100%;
}

.tab-label {
    font-size: 20px;
}

.challenge-inprogress {
    padding: 12px 24px 0;
}
.select-status {
    width: 140px;
    align-self: start;
    margin-bottom: 10px;
    margin-left: 2px;
}
.challenge-inprogress-card {
    display: flex;
    align-items: center;
    background-color: #f5f5f5;
    border-radius: 10px;
    padding-right: 10px;
}

.challenge-inprogress-card:hover {
    background-color: #ddd;
    opacity: 0.85;
}

.challenge-info {
    margin-top: 12px;
    display: flex;
    flex-direction: column;
}

.challenge-inprogress-info {
    display: flex;
    flex-grow: 1;
}

.challenge-inprogress-detail,
.challenge-inprogress-status {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.challenge-inprogress-detail {
    align-items: start;
}

.challenge-inprogress-info:hover {
    cursor: pointer;
}

.challenge-inprogress-info:hover h3 {
    color: #8231d3;
}

/* set cứng */
.challenge-inprogress-img {
    width: var(--challenge-img-width);
    height: var(--challenge-img-height);
    object-fit: cover;
    border-radius: 6px;
    margin-right: 16px;
}

.ranking-top {
    display: flex;
    flex-direction: column;
}

.ranking-title {
    align-self: center;
    margin-bottom: 6px;
}

.ranking-user {
    display: flex;
    align-items: center;
    padding: 11px;
    border-bottom: 1px solid #e8e8e8 !important;
}

.ranking-user:hover {
    cursor: pointer;
    opacity: 0.85;
}

.ranking-user:hover .ranking-user-title {
    color: #8231d3;
}

.ranking-user-avatar img {
    width: 50px;
    height: 50px;
    border-radius: 50%;
}

.ranking-user-meta {
    margin-left: 16px;
}

.ranking-user-title {
    font-size: 18px;
    font-weight: 500;
    margin-bottom: 8px;
    margin-top: 10px;
}

.ranking-user-description {
    font-size: 14px;
    color: #666;
    margin-bottom: 4px;
}

.ranking-medal {
    width: 30px;
}
</style>