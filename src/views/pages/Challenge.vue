
<template>
    <div class="challenge-container">
        <Main>
            <!-- Phần breadcrumb -->
            <BreadcrumbWrapperStyle>
                <a-breadcrumb>
                    <a-breadcrumb-item>
                        <router-link to="/">Home</router-link>
                    </a-breadcrumb-item>
                    <a-breadcrumb-item>
                        <router-link to="/thuthach">Thử thách</router-link>
                    </a-breadcrumb-item>
                </a-breadcrumb>
            </BreadcrumbWrapperStyle>
            <!-- Phần trên: Tạo thử thách -->
            <div class="create-challenge">
                <h2>Bắt đầu thử thách</h2>
                <a-row :gutter=[16,8]>
                    <a-col :xxl=" 8" :xl=" 8" :lg=" 12" :md=" 24" :sm=" 24" :xs=" 24" v-for="   item    in    challengeItems   "
                        :key=" item.title ">
                        <ChallengeCard :backgroundImage=" item.backgroundImage " :title=" item.title " :desc=" item.desc "
                            :btnText=" item.btnText " :btnClick="() => showModal(item.id)" />
                    </a-col>
                </a-row>
            </div>
            <ModalCreateChallenge :type=" modalState.type " :visible=" modalState.visible "
                :onOk="() => createThuThach(modalState.currentModalID)" :onCancel=" handleCancel "
                :modalID=" modalState.currentModalID " :formState=" modalState.formState " />
            <!-- Phần giữa: Thử thách đang diễn ra và ranking -->
            <div class="challenge-info">
                <a-row :gutter= [24,8]>
                    <a-col :xxl=" 18" :xl=" 18" :lg=" 24" :md=" 24" :sm=" 24" :xs=" 24">
                        <div class="challenge-inprogress">
                            <h2>Thử thách đang diễn ra</h2>
                            <TabBasic v-model:activeKey=" activeKey ">
                                <Child v-for="(  tab, index  ) in   tabs  " :key=" index + 1">
                                    <template #tab><span class="tab-label">{{ tab.label }}</span></template>
                                    <div class="select-status">
                                        <a-select v-if=" index == 0" v-model:value=" selectState " style="width: 100%"
                                            @change=" handleChangeSelectStatus ">
                                            <a-select-option value="happening">Đang diễn ra</a-select-option>
                                            <a-select-option value="upcoming">Sắp diễn ra</a-select-option>
                                            <a-select-option value="ended">Đã diễn ra</a-select-option>
                                        </a-select>
                                        <div v-else style="height: 2.375rem;"></div>
                                    </div>
                                    <div v-if=" activeKey === tab.id" class="tab-content">
                                        <a-row :gutter= [16,16] v-if=" tab.id === 1 || tab.id === 2 " class="tab-list">
                                            <a-col :xxl=" 12" :xl=" 24" :lg=" 24" :md=" 24" :sm=" 24" :xs=" 24" v-for="(  item, index  ) in (tab.id === 1 && selectState === 'happening' ? displayHappeningTour :
                                                tab.id === 1 && selectState === 'upcoming' ? displayUpcomingTour :
                                                    tab.id === 1 && selectState === 'ended' ? displayEndedTour :
                                                        tab.id === 2 ? displayedSoloItems : [])" :key=" index ">
                                                <div class="challenge-inprogress-card">
                                                    <div class="challenge-inprogress-info">
                                                        <img :src=" item.poster " alt="Poster"
                                                            class="challenge-inprogress-img" />
                                                        <div class="challenge-inprogress-detail">
                                                            <h3 class="truncate-text">{{ item.title }}</h3>
                                                            <p>{{ item.startTime }}</p>
                                                        </div>
                                                    </div>
                                                    <span class="challenge-inprogress-status">{{ item.status }}</span>
                                                </div>

                                            </a-col>
                                        </a-row>
                                    </div>
                                    <div class="pagination-wrapper">
                                        <sdCards v-show=" tab.id == 1 && selectState == 'happening' "
                                            class="challenge-pagination">
                                            <a-pagination v-model=" currentPageState.tournament.happening "
                                                :total=" happeningTourItems " :showSizeChanger=" false"
                                                :pageSize=" tournamentPageSize " show-less-items
                                                @change=" handleHappeningPage " />
                                        </sdCards>
                                        <sdCards v-show=" tab.id == 1 && selectState == 'upcoming' "
                                            class="challenge-pagination">
                                            <a-pagination v-model=" currentPageState.tournament.upcoming "
                                                :total=" upcomingTourItems " :showSizeChanger=" false"
                                                :pageSize=" tournamentPageSize " show-less-items
                                                @change=" handleUpcomingPage " />
                                        </sdCards>
                                        <sdCards v-show=" tab.id == 1 && selectState == 'ended' "
                                            class="challenge-pagination">
                                            <a-pagination v-model=" currentPageState.tournament.ended "
                                                :total=" endedTourItems " :showSizeChanger=" false"
                                                :pageSize=" tournamentPageSize " show-less-items
                                                @change=" handleEndedChange " />
                                        </sdCards>
                                        <sdCards v-show=" tab.id == 2 " class="challenge-pagination">
                                            <a-pagination v-model=" currentPageState.solo.current "
                                                :total=" soloTotalItems " :showSizeChanger=" false"
                                                :pageSize=" soloPageSize " show-less-items
                                                @change=" handleSoloPageChange " />
                                        </sdCards>
                                    </div>
                                </Child>
                            </TabBasic>
                        </div>
                    </a-col>
                    <a-col :xxl=" 6" :xl=" 6" :lg=" 24" :md=" 24" :sm=" 24" :xs=" 24">
                        <div class="ranking-top">
                            <div class="ranking-title">
                                <h2 style="margin-bottom: 8px;">Bảng xếp hạng</h2>
                            </div>
                            <a-list :item-layout=" 'horizontal'" :dataSource=" selectTopRankingUsers ">
                                <template #renderItem="{ item }">
                                    <div class="ranking-user"
                                        :style="{ 'border-color': getRankingStyle(item.order).color }">
                                        <div class="ranking-user-avatar">
                                            <img :src=" item.avatar " alt="Avatar" />
                                        </div>
                                        <div class="ranking-user-meta">
                                            <h3 class="ranking-user-title"
                                                :style="{ color: getRankingStyle(item.order).color }">
                                                <img v-if=" getRankingStyle(item.order).icon "
                                                    :src=" getRankingStyle(item.order).icon " alt="Ranking Icon"
                                                    class="ranking-medal" />
                                                <span v-else>{{ `${ item.order }.` }}</span>
                                                {{ item.name }}
                                            </h3>
                                            <p class="ranking-user-description">{{ `Elo: ${ item.elo } | Attended:
                                                ${ item.attended }` }}</p>
                                        </div>
                                    </div>
                                </template>
                            </a-list>
                            <Buttons class="btn-ranking-view" type="primary" @click=" toggleViewMore ">{{ viewMore ? 'Ẩn bớt'
                                : 'Xem thêm' }}</Buttons>
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
import ModalCreateChallenge from "./ModalCreateChallenge.vue";
import { TabBasic, Child } from "@/components/tabs/Style";
import { ref, computed, onMounted, reactive } from 'vue';

//props 

// DATA
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
    solo: Array.from({ length: 70 }, (_, index) => ({
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
    type: 'primary',
});
const tournamentFormState = reactive({
    challengeName: '',
    shortDesc: '',
    start: '',
    end: '',
    level: 'easy',
    challengeTime: 'time1',
    apiUpload: '/api/upload',
    bannerFile: [],
});
const soloFormState = reactive({
    challengeName: '',
    level: 'easy',
    challengeTime: 'time1',

});
const trainFormState = reactive({
    challengeName: '',
    level: 'easy',
    challengeTime: 'time1',

});
let activeKey = ref(1);

let selectState = ref('happening');
const currentPageState = reactive({
    tournament: {
        happening: 1,
        upcoming: 1,
        ended: 1
    },
    solo: {
        current: 1
    }
})
// let soloCurrentPage = ref(1);
const tournamentPageSize = ref(12);
const soloPageSize = ref(12);
let viewMore = ref(false);

// MOUNTED
onMounted(() => {


});
//COMPUTED

const happeningTourItems = computed(() => {
    const items = challengeInProgress.tournament;
    return items.filter(item => item.status === 'happening').length;
});
const upcomingTourItems = computed(() => {
    const items = challengeInProgress.tournament;
    return items.filter(item => item.status === 'upcoming').length;
});
const endedTourItems = computed(() => {
    const items = challengeInProgress.tournament;
    return items.filter(item => item.status === 'ended').length;
});
const soloTotalItems = computed(() => {
    const items = challengeInProgress.solo;
    return items.length;
});
const displayHappeningTour = computed(() => {
    const currentPage = currentPageState.tournament.happening;
    const items = challengeInProgress.tournament;
    const startIndex = (currentPage - 1) * soloPageSize.value;
    const endIndex = startIndex + soloPageSize.value;
    return items.filter(item => item.status === 'happening').slice(startIndex, endIndex);
});

const displayUpcomingTour = computed(() => {
    const currentPage = currentPageState.tournament.upcoming;
    const items = challengeInProgress.tournament;
    const startIndex = (currentPage - 1) * soloPageSize.value;
    const endIndex = startIndex + soloPageSize.value;
    return items.filter(item => item.status === 'upcoming').slice(startIndex, endIndex);
});

const displayEndedTour = computed(() => {
    const currentPage = currentPageState.tournament.ended;
    const items = challengeInProgress.tournament;
    const startIndex = (currentPage - 1) * soloPageSize.value;
    const endIndex = startIndex + soloPageSize.value;
    return items.filter(item => item.status === 'ended').slice(startIndex, endIndex);
});


const displayedSoloItems = computed(() => {
    const items = challengeInProgress.solo;
    const startIndex = (currentPageState.solo.current - 1) * soloPageSize.value;
    const endIndex = startIndex + soloPageSize.value;
    return items.slice(startIndex, endIndex);
});

let selectTopRankingUsers = computed(() => {
    return viewMore.value ? topRankingUsers : topRankingUsers.slice(0, 10);
});

//METHODS

const showModal = (id: number) => {
    modalState.currentModalID = id;
    modalState.visible = true;
}
const createThuThach = (id: number) => {
    switch (id) {
        case 1:
            const createTournamentData = {
                challengeName: tournamentFormState.challengeName,
                shortDesc: tournamentFormState.shortDesc,
                start: tournamentFormState.start,
                end: tournamentFormState.end,
                level: tournamentFormState.level,
                challengeTime: tournamentFormState.challengeTime,
                bannerFile: tournamentFormState.bannerFile,
            }
            console.log(createTournamentData);
            break;
        case 2:
            const createSoloData = {
                challengeName: soloFormState.challengeName,
                level: soloFormState.level,
                challengeTime: soloFormState.challengeTime,
            }
            console.log(createSoloData);
            break;
        case 3:
            const createTrainData = {
                challengeName: trainFormState.challengeName,
                level: trainFormState.level,
                challengeTime: trainFormState.challengeTime,
            }
            console.log(createTrainData);
            break;
        default:
            break;
    }

    setTimeout(() => {
        modalState.visible = false;
    }, 1000);
}
const handleCancel = () => {
    modalState.visible = false;
}
const handleHappeningPage = (page: any) => {
    currentPageState.tournament.happening = page;
};
const handleUpcomingPage = (page: any) => {
    currentPageState.tournament.upcoming = page;
};
const handleEndedChange = (page: any) => {
    currentPageState.tournament.ended = page;
};
const handleSoloPageChange = (page: any) => {
    currentPageState.solo.current = page;
};
const toggleViewMore = () => {
    viewMore.value = !viewMore.value;
}
const handleChangeSelectStatus = () => {
}

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
/* css breadcrumb mặc định */
:global(.ant-breadcrumb .ant-breadcrumb-link a) {
    color: black !important;
}

:global(.ant-breadcrumb .ant-breadcrumb-link .router-link-active) {
    color: #8231D3 !important;
}

/* css pagination */
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

:global(.truncate-text) {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    overflow: hidden;
    -webkit-line-clamp: 2;
    /* Hiển thị tối đa 2 dòng */
    text-overflow: ellipsis;
}

.challenge-container {
    padding: 16px 30px 0;
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
    min-height: 112vh;
}

.tab-content {
    flex-grow: 1;
    width: 100%;
}

.tab-label {
    font-size: 1.25rem;
}

.challenge-inprogress {
    padding: 12px 24px 0;
}

.select-status {
    width: 8.75rem;
    align-self: flex-start;
    margin-bottom: 10px;
    margin-left: 2px;
}

.challenge-inprogress-card {
    display: flex;
    align-items: center;
    background-color: #f5eefb;
    border-radius: 10px;
    padding-right: 10px;
    border: 1px solid #d5acfa;
}

.challenge-inprogress-card:hover {
    background-color: #f1e4fc;
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


.challenge-inprogress-img {
    display: block;
    background-image: url("https://media.istockphoto.com/id/1308682666/vector/blue-gradient-soft-background.jpg?s=612x612&w=0&k=20&c=CBSD2BDe2uMi-Zm65ny6KoPKXsTPdk5K8wt_vMIb3Hc=");
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
    width: 3.125rem;
    height: 3.125rem;
    border-radius: 50%;
}

.ranking-user-meta {
    margin-left: 16px;
}

.ranking-user-title {
    font-size: 1.125rem;
    font-weight: 500;
    margin-bottom: 8px;
    margin-top: 10px;
}

.ranking-user-description {
    font-size: 0.875rem;
    color: #666;
    margin-bottom: 4px;
}

.ranking-medal {
    width: 1.875rem;
}



/* responsive */
@media (max-width: 576px) {
    .lmeiNC {
        padding: 0;
    }
    .challenge-container {
        padding-left: 0;
        padding-right: 0;
    }

    .challenge-inprogress-img {
        height: calc((100vw - 78px) * 9 /16);
        width: 100%;
    }

    .challenge-inprogress-card {
        flex-direction: column;
        padding-right: 0;
    }

    .challenge-inprogress-info {
        width: 100%;
        flex-direction: column;
        flex-grow: 1;
    }

    .challenge-inprogress-detail {
        align-items: center;
        align-self: center;
    }
}

/* Small screens (sm) */
@media (min-width: 576px) and (max-width: 767px) {
    .lmeiNC {
        padding: 0;
    }
    .challenge-container {
        padding-left: 0;
        padding-right: 0;
    }

    .challenge-inprogress-img {
        height: calc((100vw - 78px) * 9 /16);
        width: 100%;
    }

    .challenge-inprogress-card {
        flex-direction: column;
        padding-right: 0;
    }

    .challenge-inprogress-info {
        width: 100%;
        flex-direction: column;
        flex-grow: 1;
    }

    .challenge-inprogress-detail {
        align-items: center;
        align-self: center;
    }
}

/* Medium screens (md) */
@media (min-width: 768px) and (max-width: 991px) {
    .challenge-inprogress-img {
        height: 16.875vh;
        width: 30vh;
    }
}

/* Large screens (lg) */
@media (min-width: 992px) and (max-width: 1199px) {
    .challenge-inprogress-img {
        height: 16vh;
        width: 28.4444vh;
    }
}

/* Extra-large screens (xl) */
@media (min-width: 1200px) and (max-width: 1599px) {
    .challenge-inprogress-img {
        height: 20vh;
        width: 35.5556vh;
    }
}

/* Extra-extra-large screens (xxl) */
@media (min-width: 1600px) {
    .challenge-inprogress-img {
        height: 14vh;
        width: 24.8889vh;
    }
}
</style>