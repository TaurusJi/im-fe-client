<template>
  <view-page class="add-friend" title="添加">
    <form action="/">
      <van-search v-model="searchValue" show-action placeholder="请输入搜索关键词" @search="onSearch" @cancel="onCancel" />
    </form>
    <div class="f-list">
      <friend v-for="user of users" :key="user.id" :friend="user" @add="onAdd" type="user"></friend>
      <friend v-for="group of groups" :key="group.id" :friend="group" @add="onAdd" type="group"></friend>
    </div>
  </view-page>
</template>

<script>
import friend from './compenents/Friend';

export default {
  name: 'add-friend',
  components: {
    friend
  },
  props: {},
  data() {
    return {
      searchValue: '',
      users: [],
      groups: []
    };
  },
  computed: {},
  watch: {},
  created() {},
  methods: {
    onAdd({ type, toId }) {
      const params = {
        type,
        toId
      };
      this.$http
        .post(this.$urls.add.applies, params)
        .then(data => {
          this.$toast('已申请');
        })
        .catch(error => {
          this.$toast(error.errorMessage);
        });
    },
    onSearch() {
      console.log('onSearch');
      this.$http
        .post(this.$urls.add.search, {
          searchValue: this.searchValue
        })
        .then(data => {
          this.users = data.users;
          this.groups = data.groups;
        })
        .catch(error => {
          this.$toast(error.errorMessage);
        });
    },
    onCancel() {
      console.log('onCancel');
    },
    onClickLeft() {
      this.$router.back();
    }
  }
};
</script>

<style lang="scss">
.add-friend {
  display: flex;
  position: relative;
  background-color: #fff;
  .avatar {
    width: 40px;
    height: 40px;
    margin: 10px;
    flex-shrink: 0;
    img {
      background-color: #fff;
      width: 100%;
      height: 100%;
      border-radius: 50%;
    }
  }
  .name-info {
    flex-grow: 5;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    .name {
      color: #2f2f2f;
      font-size: 18px;
    }
    .info {
      color: #b1b1b1;
      font-size: 16px;
    }
  }
  .operation {
    width: 80px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}
</style>
