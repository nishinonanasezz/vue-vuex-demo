<template>
  <div>
    <section>
      <header class="top_tips">
        <span class="num_tip" v-if="$route.path == '/'">{{ level }}</span>
        <span class="num_tip" v-if="$route.path == '/item'">题目: {{itemNum}}</span>
      </header>
    </section>

    <div v-if="$route.path == '/'">
      <div class="home_logo item_container_style"></div>
      <router-link to="item" class="btn start"></router-link>
    </div>

    <div v-if="$route.path == '/item'">
      <div class="item_back item-container_style">
        <div class="item_list_container" v-if="itemDetail.length > 0">
         <header class="item_title">{{itemDetail[itemNum-1].topic_name}}</header>
          <ul>
            <li class="item_list" v-for="(item,index) in itemDetail[itemNum-1].topic_answer" :key="index" @click="choosed(index,item.topic_answer_id)">
              <span class="option_style" :class="{'has_choosed':chooseNum==index}">{{chooseType(index)}}</span>
              <span class="option_detail">{{item.answer_name}}</span>
            </li>
          </ul>
        </div>
      </div>
      <span class="next_item btn" v-if="itemNum < itemDetail.length" @click="next"></span>
      <span class="submit_item btn" v-else @click="submitAnswer"></span>
    </div>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'
export default {
  props: ["fatherComponent"],
  data() {
    return {
      itemId: null, // 题目ID 
      chooseNum: null,  // 选中答案索引
      chooseId: null, // 选中答案id
    };
  },
  created() {
  },
  computed: {
    ...mapState([
      'itemNum', // 第几题
      'level', // 第几周
      'itemDetail', // 题目详情
      'timer', // 计时器
    ]),
  },
  watch: {
    '$route.path': {
      handler(val) {
        if(val) {
          console.log(val);
          if(val == '/') {
            this.initializeData();
          }
        }
      },
      deep:true,
      immediate: true,//第一次初始化渲染就可以监听到
    }
  },
  methods: {
     ...mapActions([
      'addNum','initializeData',
    ]),
    // 选中的答案信息
    choosed(type,id) {
      this.chooseNum = type
      this.chooseId = id
      },
      // 下一题
      next() {
        if(this.chooseNum !== null) {
          this.chooseNum = null;
          // 保存答案 题目索引加1 跳到下一题
          this.addNum(this.chooseId)
        }else {
          alert('你还没有选择答案哦~')
        }
      },
      // 提交答案 最后一题 提交 清空定时器 跳转分数页
      submitAnswer() {
        if(this.chooseNum !== null) {
           this.addNum(this.chooseId);
           clearInterval(this.timer)
           this.$router.push('score');
        }else {
          alert('你还没有选择答案哦~')
        }
      },
    chooseType:type => {
      switch(type) {
        case 0: return 'A';
        case 1: return 'B';
	  		case 2: return 'C';
	  		case 3: return 'D';
      }
    }
  }
};
</script>

<style lang="less" scoped>
.top_tips {
  position: absolute;
  height: 9rem;
  width: 4.8rem;
  top: -0.3rem;
  right: 2.6rem;
  background: url(../images/WechatIMG2.png) no-repeat;
  background-size: 100% 100%;
  z-index: 10;
  .num_tip {
    position: absolute;
    left: 1.3333rem;
    bottom: 1.3333rem;
    height: 0.8rem;
    width: 2.6667rem;
    font-size: 0.6667rem;
    font-family: "黑体";
    font-weight: 600;
    color: #f60;
    text-align: center;
  }
}
.item_container_style {
  height: 17.625rem;
  width: 18rem;
  background-repeat: no-repeat;
  position: absolute;
  top: 6.1rem;
  left: 1rem;
}
.home_logo {
  background-image: url(../images/1-2.png);
  background-size: 18rem 100%;
  background-position: right center;
}
.item_back {
  background-image: url(../images/2-1.png);
  background-size: 100% 100%;
}
.start {
  background-image: url(../images/1-4.png);
}
.btn {
  position: absolute;
  left: 50%;
  top: 24rem;
  transform: translate(-50%);
  display: block;
  height: 2.6667rem;
  width: 5.3333rem;
  background-size: 100% 100%;
  background-repeat: no-repeat;
}
.next_item {
    background-image: url(../images/2-2.png);
  }
.submit_item {
  background-image: url(../images/3-1.png);
}
.item_list_container{
    position: absolute;
    height: 10rem;
    width: 10rem;
    top: 11rem;
    left: 7rem;
  -webkit-font-smoothing: antialiased;
  }
.item_title{
		font-size: 1.0667rem;
		color: #f60;
		line-height: 0.7rem;
	}
.item_list{
		font-size: 0;
		margin-top: .8rem;
		width: 10rem;
		span{
			display: inline-block;
			font-size: 1.0667rem;
			color: #f60;
			vertical-align: middle;
		}
		.option_style{
			height: 0.725rem;
			width: 0.725rem;
			border: 1px solid #fff;
			border-radius: 50%;
			line-height: 0.725rem;
			text-align: center;
			margin-right: .4267rem;
			font-size: .8rem;
			font-family: 'Arial';
		}
		.has_choosed{
			background-color: #ffd400;
			color: #333;
			border-color: #ffd400;
		}
		.option_detail{
			width: 7.5rem;
			padding-top: 0.11rem;
		}
	}
</style>
