<template>
  <div class="wrapper">
    <div class="news">
      <div class="news-title">
        <h2>公告事項</h2>
      </div>
      <div class="news-content" v-for="item in newsList" :key="item.NewsId">
        <p class="news-date">{{ item.NewsAddDate }}</p>
        <h3 class="news-subject" @click="open(item.NewsId)">{{ item.NewsTitle }}</h3>
        <button type="button" id="open" class="news-more" @click="open(item.NewsId)">
          ...more
        </button>
      </div>
      
      <div class="news-pagination">
        <span
        v-if="page > 1 && list.length > 0"
        class="pages_button_prev"
        @click="setPage(page - 1)"
        >
        &lt;
        </span>
        <span
          v-for="p in pages"
          :key="p.id"
          @click="setPage(p)"
          :class="{ pages_button: true, 'pages_button--active': p == page }"
        >
          {{ p }}
        </span>
        <span
          v-if="page < pages.length && list.length > 0"
          class="pages_button_next"
          @click="setPage(page + 1)"
        >
          &gt;
        </span>
      </div>
    </div>
  </div>

  <!-- 燈箱 -->
  <div class="popup-mask" :style="modalStyle">
    <div class="popup-container" @click.self="close()">
      <div class="popup-body">
        <div class="popup-title">
          <h3 class="popup-subject">{{ popupData.NewsTitle }}</h3>
          <p class="popup-date">{{ popupData.NewsAddDate }}</p>
        </div>
        <div class="popup-content">
          <p v-html="popupData.NewsContent"></p>
          <!-- 在需要換行的地方加入<br> -->
        </div>
        <button type="button" @click="close()">關閉</button>
      </div>
    </div>
  </div>
</template>

<script>
// import axios from 'axios';
// const url = 'http://172.17.100.110:3000';

export default {
  name: "NewsList",
  data() {
    return {
      activeId: "",
      page: 1, //當前頁碼
      pages: [], //總共頁數
      perPage: 14, //每頁多少項目
      list: [
      {
        NewsId: 1,
        NewsTitle: '新產品發佈會通知',
        NewsAddDate: '2024/10/01',
        NewsContent: '我們將於10月15日舉行新產品發佈會，歡迎大家參加。',
    },
    {
        NewsId: 2,
        NewsTitle: '系統維護公告',
        NewsAddDate: '2024/10/05',
        NewsContent: '本公司將於10月20日進行系統維護，屆時服務將暫停。',
    },
    {
        NewsId: 3,
        NewsTitle: '年度員工大會通知',
        NewsAddDate: '2024/10/10',
        NewsContent: '年度員工大會將於11月5日舉行，請全體員工準時參加。',
    },
    {
        NewsId: 4,
        NewsTitle: '市場調查報告發布',
        NewsAddDate: '2024/10/15',
        NewsContent: '我們已完成最新市場調查報告，請查閱內部網頁獲取詳情。',
    },
    {
        NewsId: 5,
        NewsTitle: '安全培訓課程',
        NewsAddDate: '2024/10/20',
        NewsContent: '將於11月10日舉辦安全培訓課程，所有員工必須參加。',
    },
    {
        NewsId: 6,
        NewsTitle: '客戶回饋調查',
        NewsAddDate: '2024/10/25',
        NewsContent: '我們正在進行客戶回饋調查，請協助填寫問卷以改善服務。',
    },
    {
        NewsId: 7,
        NewsTitle: '內部升遷公告',
        NewsAddDate: '2024/11/01',
        NewsContent: '恭喜以下同仁獲得升遷，詳情請參閱內部公告。',
    },
    {
        NewsId: 8,
        NewsTitle: '年度團建活動',
        NewsAddDate: '2024/11/05',
        NewsContent: '我們將於12月1日舉辦年度團建活動，敬請期待！',
    },
    {
        NewsId: 9,
        NewsTitle: '業務拓展會議',
        NewsAddDate: '2024/11/08',
        NewsContent: '將於11月15日舉行業務拓展會議，歡迎各部門參加。',
    },
    {
        NewsId: 10,
        NewsTitle: '新員工入職培訓',
        NewsAddDate: '2024/11/10',
        NewsContent: '新員工入職培訓將於11月20日開始，請新同仁準備資料。',
    },
    {
        NewsId: 11,
        NewsTitle: '企業社會責任活動',
        NewsAddDate: '2024/11/12',
        NewsContent: '我們將於11月25日舉辦CSR活動，鼓勵員工參與社區服務。',
    },
    {
        NewsId: 12,
        NewsTitle: '網站更新公告',
        NewsAddDate: '2024/11/15',
        NewsContent: '公司網站將於11月30日進行更新，請大家留意。',
    },
    {
        NewsId: 13,
        NewsTitle: '季度業績報告會',
        NewsAddDate: '2024/11/18',
        NewsContent: '季度業績報告會將於12月5日舉行，歡迎大家參加。',
    },
    {
        NewsId: 14,
        NewsTitle: '健康檢查通知',
        NewsAddDate: '2024/11/20',
        NewsContent: '公司將於12月10日提供健康檢查服務，請提前報名。',
    },
    {
        NewsId: 15,
        NewsTitle: '技能提升工作坊',
        NewsAddDate: '2024/11/22',
        NewsContent: '我們將於12月15日舉辦技能提升工作坊，提升員工專業能力。',
    },
    {
        NewsId: 16,
        NewsTitle: '冬季聚餐活動',
        NewsAddDate: '2024/11/25',
        NewsContent: '冬季聚餐將於12月20日舉行，請全體員工報名參加。',
    },
    {
        NewsId: 15,
        NewsTitle: '技能提升工作坊',
        NewsAddDate: '2024/11/22',
        NewsContent: '我們將於12月15日舉辦技能提升工作坊，提升員工專業能力。',
    },
    {
        NewsId: 16,
        NewsTitle: '冬季聚餐活動',
        NewsAddDate: '2024/11/25',
        NewsContent: '冬季聚餐將於12月20日舉行，請全體員工報名參加。',
    }
      ], //當前顯示項目
      newsList: [],
      // Content:[],
    };
  },
  methods: {
    open(id) {
      this.activeId = id;
    },
    close() {
      this.activeId = NaN;
    },
    setPage(p) {
    if (p !== this.page) {
      this.page = p;
      let minI = this.perPage * (this.page - 1); // 計算起始索引
      let maxI = this.perPage * this.page; // 計算結束索引
      this.newsList = this.list.slice(minI, maxI); // 只顯示當前頁的資料
    }
  },
},
  computed: {
    modalStyle() {
      return {
        display: this.activeId ? "" : "none",
      };
    },
    popupData() {
      return this.list.find((n) => n.NewsId === this.activeId) || {};
    },

  },
  mounted() {
    // axios.get(`${url}/news`)
    //   .then((res) => {
    //     const filteredData = res.data.data.filter(data => data.NewsStatus === 1);
    //     this.newsList = filteredData.slice(0, 28);
        // console.log(this.newsList);
        let pagesAmount = Math.ceil(this.list.length / this.perPage);
        for (let i = 1; i <= pagesAmount; i++) {
          this.pages.push(i);
        }
        this.setPage(1);
        this.newsList = this.list.slice(0, this.perPage); // 初始化第一頁的資料
        // })
        // .catch(err => {
        //   console.dir(err);
        // });
},
}
</script>

<style scoped lang="scss">
@import "@/assets/css/app.scss";

.wrapper {
  width: 100%;
  margin: 10px;
  box-sizing: border-box;
  @include pad-width() {
    width: 90%;
  }
  @include pc-l-width() {
    width: 50%;
    height: 450px;
  }

  .news {
    @include news-border;
    height: 440px;
    position: relative;
    .news-title {
      h2 {
        font-size: 1.2em;
        text-align: left;
        padding: 5px 20px;
        border-bottom: $border;
      }
    }
    .news-content {
      display: flex;
      align-items: center;
      margin: 5px 20px;
      padding-bottom: 2px;
      border-bottom: 1px solid black;
      .news-date {
        font-size: 0.9em;
        margin: 0 10px;
        font-weight: normal;
      }
      .news-subject {
        font-size: 1em;
        cursor: pointer;
      }
      .news-more {
        margin-left: auto;
        margin-right: 10px;
        color: $secondary;
        cursor: pointer;
      }
    }
    .news-pagination {
      display: flex;
      justify-content: center;
      margin: 5px;
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      .pages_button, .pages_button_prev, .pages_button_next {
        cursor: pointer;
        font-size: 0.9em;
        background-color: $light;
        border: 2px solid $primary;
        color: $primary;
        padding: 5px 9px;
        border-radius: 50%;
        margin: 5px;
      }
  
      .pages_button:hover, .pages_button--active {
        background-color: $primary;
        color: #fff;
      }
    }
  }
}

//燈箱
.popup-mask {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  display: table;
  backdrop-filter: blur(2px);
  z-index: 10;

  .popup-container {
    display: table-cell;
    vertical-align: middle;
    .popup-body {
      width: 80%;
      margin: auto;
      padding: 20px 30px 20px 20px;
      @include popup-border;
      @include pc-l-width() {
        width: 60%;
      }
      @include pc-width() {
        width: 45%;
      }
      .popup-title {
        border-bottom: 2px solid $primary;
        display: flex;
        align-items: center;
        padding-bottom: 5px;
        justify-content: space-between;

        .popup-subject {
          font-size: 1em;
          @include pad-width() {
            font-size: 1.3em;
          }
          @include pc-l-width() {
            font-size: 1.5em;
          }
        }
        .popup-date {
          font-size: 0.9em;
          margin: 0 10px;
        }
      }
      .popup-content {
        width: 100%;
        height: 400px;
        margin: 20px 10px;
        text-align: left;
        text-indent: 2em;
        line-height: 1.5;
        font-size: 0.9em;
        overflow-y: scroll;
      }
      button {
        cursor: pointer;
        color: $light;
        background-color: $primary;
        margin: auto;
        padding: 5px 20px;
        border-radius: 15px;
      }
    }
  }
}

//捲軸寬度
::-webkit-scrollbar {
  width: 5px;
}

//捲軸底色
// ::-webkit-scrollbar-track {
// }

//捲軸本體顏色
::-webkit-scrollbar-thumb {
  background: #dbeff9a3;
  border-radius: 10px;
}
</style>
