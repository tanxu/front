<template>
  <div>
    <ul class="fly-list">
      <li v-for="(item,index) in items" :key="'listitem'+index">
        <a href="user/home.html" class="fly-avatar">
          <img src="https://tva1.sinaimg.cn/crop.0.0.118.118.180/5db11ff4gw1e77d3nqrv8j203b03cweg.jpg" alt="贤心">
        </a>
        <h2>
          <a class="layui-badge">{{ item.catalogstr }}</a>
          <a href="jie/detail.html">{{ item.title }}</a>
        </h2>
        <div class="fly-list-info">
          <a href="user/home.html" link>
            <cite>{{ item.user.username }}</cite>
            <!--            <i class="iconfont icon-renzheng" title="认证信息：XXX"></i>-->
            <i class="layui-badge fly-badge-vip" v-if="item.user.isVip!=='0'">VIP{{ item.user.isVip }}</i>
          </a>
          <span>{{ item.create_time | moment }}</span>

          <span class="fly-list-kiss layui-hide-xs" title="悬赏飞吻"><i class="iconfont icon-kiss"></i> {{
              item.fav
            }}</span>
          <span class="layui-badge fly-badge-accept layui-hide-xs" v-show="item.status!==0">已结</span>
          <span class="fly-list-nums">
            <i class="iconfont icon-pinglun1" title="回答"></i> {{ item.answer }}
          </span>
        </div>
        <div class="fly-list-badge" v-show="item.tags.length>0">
          <span class="layui-badge layui-bg-red" v-for="(tag, tagIndex) in item.tags" :key="'tag'+tagIndex"
                :class="tag.class">{{ tag.name }}</span>
        </div>
      </li>
    </ul>
    <div style="text-align: center" v-show="isShow">
      <div class="laypage-main" v-if="!isEnd">
        <a @click.prevent="more" class="laypage-next">更多求解</a>
      </div>
      <div class="nomore gray" v-else>
        没有更多了
      </div>
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import moment from 'moment'
import 'moment/locale/zh-cn'

export default {
  name: 'ListItem',
  props: {
    isEnd: {
      default: false,
      type: Boolean
    },
    lists: {
      default: () => [],
      type: Array
    },
    isShow: {
      default: true,
      type: Boolean
    }
  },
  computed: {
    items () {
      _.map(this.lists, (item) => {
        switch (item.catalog) {
          case 'ask':
            item.catalogstr = '提问'
            break
          case 'share':
            item.catalogstr = '分享'
            break
          case 'logs':
            item.catalogstr = '动态'
            break
          case 'notice':
            item.catalogstr = '公告'
            break
          case 'advise':
            item.catalogstr = '建议'
            break
          case 'discuss':
            item.catalogstr = '交流'
            break
        }
      })
      return this.lists
    }
  },
  methods: {
    more () {
      this.$emit('nextPage')
    }
  },
  filters: {
    moment (date) {
      // 超过7天显示日期
      if (moment(date).isBefore(moment().subtract(7, 'days'))) {
        return moment(date).format('YYYY-MM-DD')
      } else {
        // 小于7天显示, 1小时前, xx小时前, xx天前
        return moment(date).from(moment())
      }
    }
  }
}
</script>

<style lang="less" scoped>
.nomore {
  font-size: 16px;
  padding: 30px 0;
}
</style>
