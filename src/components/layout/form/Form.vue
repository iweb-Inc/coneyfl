<template>
  <form @submit.prevent="submitForm" name="frm">
          
    <Button @click="submitForm()" class="btn_wrap">제출하기</Button>

    <div v-if="isLoading" class="loading-modal">
      <div class="container">
        <div class="dot dot-1"></div>
        <div class="dot dot-2"></div>
        <div class="dot dot-3"></div>
      </div>

      <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
        <defs>
          <filter id="goo">
            <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
            <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 21 -7"/>
          </filter>
        </defs>
      </svg>
    </div>

    <Modal v-if="this.openModal == true">
      <template v-slot:text01>제출이 완료되었습니다!</template>
      <template v-slot:text02
        >회원가입하면 코니 활동 안내를 받으실 수 있습니다.</template
      >
      <template v-slot:btn>
        <router-link to="/signup">
          <Button>회원가입 바로가기</Button>
        </router-link>
      </template>
    </Modal>
  </form>
</template>

<script>
import FormItem from "@/components/layout/form/FormItem.vue";
import FormInput from "@/components/common/FormInput.vue";
import CheckBoxStyle01 from "@/components/common/CheckBoxStyle01.vue";
import CheckBoxStyle02 from "@/components/common/CheckBoxStyle02.vue";
import Radio from "@/components/common/Radio.vue";
import FileInput from "@/components/common/FileInput.vue";
import FileInput3 from "@/components/common/FileInput3.vue";
import SelectBox from "@/components/common/SelectBox.vue";
import Button from "@/components/common/Button.vue";
import Modal from "@/components/layout/form/Modal.vue";

import VueDatePicker from "vue3-datepicker";

//import axios from "axios";

export default {
  data() {
    return {
      isLoading: false,
      openModal: false,
      user: {
        nickname: "",
        circle: [],
        carrer: [],
        skil: [
          {
            name: "",
            level: [],
          },
        ],
        workTime: "",
        workload: "",
        income: "",
        date: new Date(),
        area: "",
        portfolioNum: "",
        portfoliFile: [],
        resume: [],
        why: "",
        job: "",
        strength: "",
        challenge: "",
        goal: "",
        mbti: "",
        path: "",
      },
      circleList: [
        "2D 모션그래픽 디자이너",
        "촬영자(컷편집 포함)",
        "유튜브 편집자",
        "라이브PD",
        "작가",
        "기획자",
        "애니메이터",
        "3D 아티스트",
      ],
      carrerList: [
        "신입",
        "2년차 이하",
        "3~5년차 이하",
        "6~10년차 이하",
        "11년차 이상",
      ],
      agreeList: [
        "agree01",
      ],
      agreeList02: [
        "agree02",
      ],
      skilList: [
        {
          key: "1",
          value: "Premiere Pro",
        },
        {
          key: "2",
          value: "Final Cut Pro",
        },
        {
          key: "3",
          value: "Davinci Resolve",
        },
        {
          key: "4",
          value: "After Effects",
        },
        {
          key: "5",
          value: "Motion",
        },
        {
          key: "6",
          value: "Fusion",
        },
        {
          key: "7",
          value: "Blender 3D",
        },
        {
          key: "8",
          value: "MAYA",
        },
        {
          key: "9",
          value: "3ds MAX",
        },
        {
          key: "10",
          value: "Cinema4D",
        },
        {
          key: "11",
          value: "시네마캠(ex. Sony FX6 이상)",
        },
        {
          key: "12",
          value: "EFP캠 이상",
        },
        {
          key: "13",
          value: "DSLR",
        },
        {
          key: "14",
          value: "디지털캠코더",
        },
        {
          key: "15",
          value: "Blackmagic ATEM",
        },
        {
          key: "16",
          value: "Vmix",
        },
        {
          key: "17",
          value: "Photoshop",
        },
        {
          key: "18",
          value: "Illustrator",
        },
        {
          key: "19",
          value: "Adobe XD",
        },
      ],
      workTimeList: [
        {
          key: "01",
          value: "평일+주말(하루 8시간 이상)",
        },
        {
          key: "02",
          value: "평일만(하루 8시간 이상)",
        },
        {
          key: "03",
          value: "주말만(하루 8시간 이상)",
        },
        {
          key: "04",
          value: "평일 저녁만(하루 4시간 이상)",
        },
        {
          key: "05",
          value: "1주일 총합 14시간 정도는 할애 가능",
        },
        {
          key: "06",
          value: "1주일 총합 14시간 미만이지만 7시간이상은 할애 가능",
        },
      ],
      workloadList: [
        {
          key: "01",
          value: "시간이 되는 한 가능한 한 많이",
        },
        {
          key: "02",
          value: "하루에 딱 시간을 정하면 그만큼만",
        },
        {
          key: "03",
          value: "적은 양을 꾸준히 하고 싶다",
        },
        {
          key: "04",
          value: "틈틈이 시간 날 때 하고 싶다",
        },
      ],
      incomeList: [
        {
          key: "01",
          value: "월 500만원 이상",
        },
        {
          key: "02",
          value: "300이상 ~ 500만원 미만",
        },
        {
          key: "03",
          value: "200이상 ~ 300만원 미만",
        },
        {
          key: "04",
          value: "50이상 ~ 100만원 미만",
        },
        {
          key: "05",
          value: "50만원 이하여도 상관없다.",
        },
      ],
      areaList: [
        { key: "01", value: "서울/경기" },
        { key: "02", value: "강원도" },
        { key: "03", value: "경상도" },
        { key: "04", value: "충청도" },
        { key: "05", value: "제주도" },
        { key: "06", value: "전라도" },
        { key: "07", value: "기타" },
      ],
      portfolioNumList: [
        {
          key: "01",
          value: "없음",
        },
        {
          key: "02",
          value: "1~5개",
        },
        {
          key: "03",
          value: "6~10개",
        },
        {
          key: "04",
          value: "11~30개",
        },
        {
          key: "05",
          value: "30~50개",
        },
        {
          key: "06",
          value: "50개 이상",
        },
      ],
      showFileInput: true,
      whyList: [
        {
          key: "01",
          value: "영상제작으로 돈을 많이 벌고 싶어서",
        },
        {
          key: "02",
          value: "돈보다는 영상제작 경험과 레퍼런스가 필요해서",
        },
        {
          key: "03",
          value:
            "지금 하고 있는 영상제작일보다 더 다양한 일을 하면서 성장하고 싶어서",
        },
        {
          key: "04",
          value: "구속받지 않고 좀 더 자유롭게 시간을 활용하고 싶어서",
        },
        {
          key: "05",
          value:
            "돈도 많이 버고 경험과 레퍼런스도 많이 쌓아 성장도 하고 싶어서",
        },
        {
          key: "06",
          value: "기타",
        },
      ],
      jobList: [
        {
          key: "01",
          value: "프리랜서",
        },
        {
          key: "02",
          value: "영상회사 근무",
        },
        {
          key: "03",
          value: "일반 직장 근무",
        },
        {
          key: "04",
          value: "대학생",
        },
        {
          key: "05",
          value: "고등학생",
        },
        {
          key: "06",
          value: "학원생",
        },
        {
          key: "07",
          value: "기타",
        },
      ],
      mbti: [
        "ENFJ",
        "ENFP",
        "ENTJ",
        "ENTP",
        "ESFJ",
        "ESFP",
        "ESTJ",
        "ESTP",
        "INFJ",
        "INFP",
        "INTJ",
        "INTP",
        "ISFJ",
        "ISFP",
        "ISTJ",
        "ISTP",
      ],

      files: [{ name: "" }],
      showAreaFormInput: false,
      showWhyFormInput: false,
    };
  },

  components: {
    FormItem,
    FormInput,
    VueDatePicker,
    CheckBoxStyle01,
    CheckBoxStyle02,
    Radio,
    FileInput,
    FileInput3,
    SelectBox,
    Button,
    Modal,
  },

  methods: {
    whySelected(event) {
      let elName = event.target.name;

      switch (elName) {
        case "why":
          if (elName == "why" && event.target.value == "기타") {
            this.showWhyFormInput = true;
          } else {
            this.showWhyFormInput = false;
          }
          break;
        case "area":
          if (elName == "area" && event.target.value == "기타") {
            this.showAreaFormInput = true;
          } else {
            this.showAreaFormInput = false;
          }
          break;
      }
    },
    nicknameChange(value) {
      this.user.nickname = value;
    },
    chceckboxSeleted(checked, val) {
      // console.log(checked, val);
      if (checked) {
        this.user.circle.push(val);
      } else {
        this.user.circle.splice(this.user.circle.indexOf(val), 1);
      }
    },
    onCarrerChanged(checked, val) {
      if (checked) {
        this.user.carrer.push(val);
      } else {
        this.user.carrer.splice(this.user.carrer.indexOf(val), 1);
      }
    },
    workTimeChanged(value) {
      this.user.workTime = value;
    },

    workLoadChanged(value) {
      this.user.workload = value;
    },
    incomeChanged(value) {
      this.user.income = value;
    },
    AreaChanged(value) {
      this.user.area = value;
    },
    portfolioNumChanged(value) {
      this.modelValue = value;
      this.showFileInput = value !== "없음";
    },
    WhyChanged(value) {
      this.user.why = value;
    },
    JobChanged(value) {
      this.user.job = value;
    },
    mbtiChanged(value) {
      this.user.mbti = value;
    },
    skilChecked(value, index) {
      this.user.skil[index] = value;
    },
    changedFilie(value, index) {
      // console.log(value);
      this.user.portfoliFile.push(value);
    },
    removeFiles(index) {
      this.user.portfoliFile.splice(index, 1);
    },
    changedFilie2(value, index) {
      this.user.resume.push(value);
    },
    removeFiles2(index) {
      this.user.resume.splice(index, 1);
    },
    submitForm(event) {
      console.log(this.user);
      /*
      
      //유효성 검사

      const nicknameRef = this.$refs.nicknameRef.$refs.nicknameInput;

      if (this.user.nickname == "") {
        nicknameRef.focus();

        return;
      }

      if (this.user.circle.length < 1) {
        const chk = document.getElementsByName("circle");
        chk[0].focus();
      }
      if (this.user.carrer.length < 1) {
        const chk = document.getElementsByName("carrer");
        chk[0].focus();
      }
     
      if (this.user.skil.length < this.skilList.length) {
        for (let j = 1; j < this.skilList.length; j++) {
          const checkboxes = document.querySelectorAll(
            `input[type="checkbox"][name="skil${j}"]`
          );
          let checkedCount = 0;

          for (let i = 0; i < checkboxes.length; i++) {
            if (checkboxes[i].checked) {
              checkedCount++;
            }
          }

          if (checkedCount === 0) {
            checkboxes[0].focus();
          }
        }
      }
    
      if (this.user.workTime == "") {
        document.getElementsByName("workTime")[0].focus();
      }
      if (this.user.workload == "") {
        document.getElementsByName("workload")[0].focus();
      }
      if (this.user.income == "") {
        document.getElementsByName("income")[0].focus();
      }

      if (this.user.date == "") {

      }

      if (this.user.area == "") {
        document.getElementsByName("area")[0].focus();
      }
      if (this.user.portfolioNum == "") {
        document.getElementsByName("portfolioNum")[0].focus();
      }

      if (this.user.portfoliFile.length < 3) {
        document.getElementsByName("portfolio")[0].focus();
      }

      if (this.user.resume.length < 3) {
        document.getElementsByName("resume")[0].focus();
      }
       if (this.user.path == "") {
        document.getElementsByName("path")[0].focus();
      }
      
      */

      this.isLoading = true; // 로딩 상태 활성화

      // setTimeout을 이용하여 2초 후에 모달 오픈
      setTimeout(() => {
        this.isLoading = false; // 로딩 상태 비활성화
        this.openModal = true; // 모달 활성화
      }, 4000);

      /* 
     axios
        .post("http://localhost:5173", this.user)
        .then(function (response) {
          console.log("Response", response);
        })
        .catch(function (err) {
          console.log("Error", err);
        });
        */
    },
  },
};
</script>

<style scoped>
.q_row_style01 {
  width: 100%;
  height: 67px;
  display: flex;
  align-items: center;
  padding: 12px 20px;
  box-sizing: border-box;
  margin: 12px 0;
  background-color: #f3f4fe;
  border-radius: 8px;
}

/*date*/
.v3dp__datepicker {
  width: 50%;
  display: flex;
  align-items: center;
  gap: 16px;
  position: relative;
}
.term_wrap > div {
  margin-bottom: 24px;
}
.btn_wrap {
  margin: 100px auto 200px;
}
.loading-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000; /* 조정 가능 */
}

.container {
  width: 200px;
  height: 200px;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  filter: url('#goo');
  animation: rotate-move 2s ease-in-out infinite;
}
.dot { 
  width: 70px;
  height: 70px;
  border-radius: 80%;
  background-color: #000;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}

.dot-3 {
  background-color: #f74d75;
  animation: dot-3-move 2s ease infinite;
}

.dot-2 {
  background-color: #10beae;
  animation: dot-2-move 2s ease infinite;
}

.dot-1 {
  background-color: #ffe386;
  animation: dot-1-move 2s ease infinite;
}

@keyframes dot-3-move {
  20% {transform: scale(1)}
  45% {transform: translateY(-18px) scale(.45)}
  60% {transform: translateY(-40px) scale(.45)}
  80% {transform: translateY(-40px) scale(.45)}
  100% {transform: translateY(0px) scale(1)}
}

@keyframes dot-2-move {
  20% {transform: scale(1)}
  45% {transform: translate(-16px, 12px) scale(.45)}
  60% {transform: translate(-40px, 40px) scale(.45)}
  80% {transform: translate(-40px, 40px) scale(.45)}
  100% {transform: translateY(0px) scale(1)}
}

@keyframes dot-1-move {
  20% {transform: scale(1)}
  45% {transform: translate(16px, 12px) scale(.45)}
  60% {transform: translate(40px, 40px) scale(.45)}
  80% {transform: translate(40px, 40px) scale(.45)}
  100% {transform: translateY(0px) scale(1)}
}

@keyframes rotate-move {
  55% {transform: rotate(0deg)}
  80% {transform: rotate(360deg)}
  100% {transform: rotate(360deg)}
}

@media (max-width: 768px) {
  .v3dp__datepicker {
    width: 100%;
  }
  .q_row_style01 {
    height: 42px;
  }
}
</style>
