<template>
  <div class="draftbox">
    <BaseHeader
            :pageinfo="{ left: 'back', title: '草稿箱', rightPage: 'home',
                   needLogin: false, }"/>
    <!--<za-nav-bar>
      <div slot="left">
        <za-icon theme="primary" type="arrow-left" @click="goBack"></za-icon>
      </div>
      <div slot="title">草稿箱</div>
    </za-nav-bar>-->
    <div v-if="draftlist.length == 0" style="margin-top:20px;">
      无记录
    </div>
    <div v-for="(item, index) in draftlist" :key="index">
      <router-link :to="{ name: 'Edit', params: { id: item.id }, query: { from: 'draft' } }" class="draft-outer">
        <div class="onecard">
          <div class="onecard_title">{{item.title}}</div>
          <div class="onecard_date">{{item.create_time}}</div>
        </div>
        <za-icon theme="default" type="wrong-round-fill" style="color: #515a6e;font-size: 24px;" @click="delModal = true;delId = item.id;"></za-icon>
      </router-link>
      <!--<za-swipe-action
        :right="action1">
        <div class="onecard">
          <div class="onecard_title">{{item.title}}</div>
          <div class="onecard_date">{{item.date}}</div>
        </div>
      </za-swipe-action>-->
    </div>
    <za-confirm :visible="delModal" title="温馨提示" message="确定删除吗？" :ok="delDraft" :cancel="()=>{ delModal = false }"></za-confirm>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import { draftList, delDraft } from '@/api';

export default {
  name: 'DeaftBox',
  data() {
    return {
      draftlist: [
        {
          title: '从加密游戏到智能签名',
          date: '1月24日',
        },
        {
          title: '从加密游戏到智能签名',
          date: '1月24日',
        },
      ],
      action1: [
        {
          theme: '',
          text: '删除',
          onClick: () => console.log('右按钮1'),
        },
      ],
      delModal: false,
      delId: null,
    };
  },
  computed: {
    ...mapGetters(['currentUsername']),
    isMe() {
      const { username, currentUsername } = this;
      return username === currentUsername;
    },
  },
  methods: {
    delDraft() {
      delDraft({ id: this.delId }, () => {
        this.getDraftList();
        this.delModal = false;
      });
    },
    goBack() {
      this.$router.go(-1);
    },
    getDraftList() {
      draftList({ page: 1 }, (res) => {
        this.draftlist = res.response.data;
      });
    },
  },
  mounted() {
    this.getDraftList();
  },
};
</script>
<style scoped>
.draft-outer {
  margin: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgba(255, 255, 255, 1);
  box-shadow: 0px 2px 5px 3px rgba(233, 233, 233, 0.5);
  border-radius: 8px;
  padding:15px;
  height: 80px;
}
a {
  color: #000;
  text-decoration: none; /* no underline */
}
.onecard{
  text-align: left;
}
.onecard_title{
  font-size: 18px;
  font-weight: bolder;
}
.onecard_date{
  font-size: 11px;
  margin-top: 6px;
  opacity: 0.4;
}
.draftbox{
  background-color: #F7F7F7;
  padding-bottom: 20px;
  padding-top: 45px;
  min-height: 100%;
}
.za-swipeAction-actions-right {
  right: 0;
  background: #d45354;
  color: #ffffff;
  margin-right: 20px;
  padding: 20px;
  width: 80px;
  padding-bottom: 15px;
  /* background: rgba(255, 255, 255, 1); */
  box-shadow: 0px 2px 5px 3px rgba(233, 233, 233, 0.5);
  border-radius: 8px;
}
/* style="transition-duration: 300ms; transform: translate3d(-120px, 0px, 0px);" */
</style>
