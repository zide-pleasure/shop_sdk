<template>
<div class="page page-current content">
  <section style="margin: 0 auto;padding: 20px 0;">
    <div style="margin: 0 auto;width:55px;height:55px;">
      <img src="http://download.dl.quzhuan.me/image/redpack/img/payError.png" style="width:100%;height:100%;vertical-align:center;">
    </div>
    <p style="color:#00a9ef;font-weight: 700;font-size: 16px;text-align: center;margin: 30px auto">
      支付失败
    </p>
  </section>
  <div class="btn" @click='reCH'>重新查询</div>
  <div class="btn" @click='contu'>确定</div>
</div>
</template>


<script>
import $ from 'zepto';
export default {
  data() {
    return {
      token: this.$route.params.token,
      orderNum: this.$route.params.orderNum
    };
  },
  methods: {
    reCH() {
      this.$http.post('http://api.ubaytop.com/platform/api/v1/pay/wft/check/status', {}, {
        params: {
          orderNum: this.orderNum,
          token: this.token
        }
      }).then(
        function(res) {
          console.log(res);
          if (res.body.code === 0) {
            if (res.body.data.payRecord.status === 3) { // 支付成功
              window.location.href = 'pages/paySuccess.html';
            } else {
              window.location.reload();
            }
          } else {
            $.toast('调用失败');
          }
        });
    },
    contu() {
      window.history.go(-2);
    }
  },
  ready() {
    document.title = '支付结果';
  }
};
</script>

<style>
section {
  width: 100%;
  padding: 50px 0;
  background: #ffffff;
  margin: 0 auto;
}

.btn {
  width: 90%;
  height: 50px;
  text-align: center;
  color: #ffffff;
  background: #00a9ef;
  margin: 30px auto;
  line-height: 50px;
  border-radius: 5px;
}
</style>
