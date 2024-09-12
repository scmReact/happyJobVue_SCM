<template>
    <div>
        <canvas id="myChart"></canvas>
    </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted, watch } from "vue";
import {
    Chart,
    CategoryScale, // 카테고리 스케일
    BarElement, // 바 차트 요소
    BarController, // 바 차트 컨트롤러
    LinearScale, // y축 선형 스케일
    Title, // 차트 제목
    Tooltip, // 툴팁
    Legend // 범례
} from "chart.js";
import { useRoute } from "vue-router";
import router from "@/router";

// 반드시 Chart.js에서 필요한 모듈을 등록
Chart.register(CategoryScale, BarElement, BarController, LinearScale, Title, Tooltip, Legend);

const chartRef = ref(null);
const route = useRoute();

const getChart = () => {
    const param = new URLSearchParams({
        searchStDate: route.query.searchStDate || "",
        searchEdDate: route.query.searchEdDate || ""
    });

    axios.post("/api/executives/chart.do", param).then((res) => {
        const dataArray = res.data;

        // list 배열에서 month, total_obtain, total_order 추출
        const months = dataArray.map((item) => item.month || ""); // month 값이 없으면 빈 문자열로 처리
        const total_obtain = dataArray.map((item) => item.total_obtain || 0); // total_obtain 값이 없으면 0으로 처리
        const total_order = dataArray.map((item) => item.total_order || 0);

        if (chartRef.value) {
            chartRef.value.destroy();
        }

        const canvas = document.querySelector("#myChart");
        if (canvas) {
            const ctx = canvas.getContext("2d");

            if (ctx) {
                chartRef.value = new Chart(ctx, {
                    type: "bar",
                    data: {
                        labels: months,
                        datasets: [
                            {
                                label: "수주액",
                                data: total_obtain,
                                backgroundColor: "hsl(0.3turn 60% 45% / .7)",
                                borderColor: "hsl(0.3turn 60% 45% / .7)",
                                borderWidth: 1
                            },
                            {
                                label: "발주액",
                                data: total_order,
                                backgroundColor: "rgba(255, 99, 132, 1)",
                                borderColor: "rgba(255, 99, 132, 1)",
                                borderWidth: 1
                            }
                        ]
                    },
                    options: {
                        scales: {
                            x: {
                                title: {
                                    display: true,
                                    text: "날짜"
                                },
                                ticks: {
                                    color: "rgba(0, 0, 0, 0.7)",
                                    font: {
                                        size: 14
                                    }
                                }
                            },
                            y: {
                                title: {
                                    display: true,
                                    text: "금액"
                                },
                                ticks: {
                                    color: "rgba(0, 0, 0, 0.7)",
                                    font: {
                                        size: 14
                                    }
                                },
                                beginAtZero: false
                            }
                        }
                    }
                });
            }
        }
    });
};

// route.query 변경 감지
watch(() => route.query, getChart);

// 새로고침 시 URL 파라미터 제거
const clearURLParams = () => {
    //console.log("뭐 들어가있냐 route.query확인", route.query); // 현재 쿼리 파라미터 확인
    //userInfoSearch:router.push(queryString); 넣은 값이 route.query에 들어있음

    if (Object.keys(route.query).length > 0) {
        router.replace({
            path: route.path, // 현재 url은 유지 => url
            query: {} //뒤에 &로 붙는 쿼리만 지워 => route.query안에 값을 지워
        });
    }
};

// 컴포넌트 마운트 시 차트 생성
onMounted(() => {
    clearURLParams();
    getChart();
});
</script>

<style lang="scss" scoped></style>
