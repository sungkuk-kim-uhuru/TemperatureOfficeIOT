
<template>
   <div id="main" >
   
       <div class="backgroundDiv widthMode" >
          <img src="../assets/bg.png">
          <div class="block">
            <span class="demonstration">スライダー</span>
            <el-slider v-model="value1"></el-slider>
          </div>
          <div class="timestamp">
            <span>{{timestamp}}</span>
          </div>
          <div class="circle " v-for="(item, index) in datas" v-bind:key="index" v-bind:class="['position' + index,datas[index].color]">
            <div class="textDiv">
              <span class="temp" >{{item.temp}}<span class="miniFont">℃</span></span></br>
              <span class="humid" >{{item.humid}}<span class="miniFont">%</span></span>
            </div>
          </div>
       </div>
    </div>
</template>

<script>
import slider from './slider.vue'

export default {
  name: 'Main',
  data () {
    return {
      datas: [
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },{
          'temp' : '',
          'humid': '',
          'color' :''
        },
        {
          'temp' : '',
          'humid': '',
          'color' :''
        },

      ],
      devicePostions : [
        {
          'deviceid' : '',
          'position' : '1'
        },
        {
          'deviceid' : '',
          'position' : '2'
        },
        {
          'deviceid' : '',
          'position' : '3'
        },
        {
          'deviceid' : 'EEA12C6A7C52',
          'position' : '4'
        },
        {
          'deviceid' : '',
          'position' : '5'
        },
        {
          'deviceid' : '',
          'position' : '6'
        },
        {
          'deviceid' : 'EBD012965BEB',
          'position' : '7'
        },
        {
          'deviceid' : '',
          'position' : '8'
        },
        {
          'deviceid' : '',
          'position' : '9'
        },
        {
          'deviceid' : '',
          'position' : '11'
        },
        {
          'deviceid' : '',
          'position' : '12'
        },
        {
          'deviceid' : '',
          'position' : '13'
        },
        {
          'deviceid' : '',
          'position' : '14'
        },
        {
          'deviceid' : '',
          'position' : '15'
        },
      ],
      timestamp : '',
      media: 0,
      alarm: 0,
      value1: 50
    }
  },
  beforeMount () {
    var thisObj = this;
    this.callAjax();
    setInterval(function() {
      thisObj.callAjax();
    }, 300000);
  },
  updated() {
    console.log('updated');
  },
  methods:{
    changeColor (resdata){
      let thisObj = this;
      let index = 0;
      resdata.forEach(function(data){
        let temperature = data.temp;
        let color = temperature < 22 ?  'circle-blue' : (temperature < 24 ? 'circle-green' : 'circle-red');
        let index = thisObj.getDevicePosition(data.deviceid);
        thisObj.datas[index].color = color;
        thisObj.datas[index].temp = temperature;
        thisObj.datas[index].humid = data.humid;
        index++;
      });    
    },
    getDevicePosition (deviceId){
      for(let i = 0; i < this.devicePostions.length; i++) {
        if (this.devicePostions[i].deviceid == deviceId) {
          return i;
        }
      }
    },  
    callAjax () {
      const baseURI = 'https://kytz6k74cl.execute-api.us-east-1.amazonaws.com/tempapi/tempdata';
      this.$http.get(`${baseURI}`)
      .then((result) => {
        console.log(result);
        let resData = result.data.body;
        console.log(resData);
        this.changeColor(resData);
        this.timestamp = resData[0].timestamp;
      })

    }
  },
  conponents:{
    'slider' : slider
  }
};
</script>

<style lang="scss">

  .timestamp{
    position: absolute;
    top: 10%;
    right:0%;
  }

  #main{
    border:1px solid #ddd;
    width:100%;
    height:100%;
    position: fixed;
    text-align: center;
  }

  .backgroundDiv{
    position: relative;
    top: 50%;


    &.widthMode{
      margin: auto;
      transform: translate(0%, -50%);
      img{
        width:100%;
      }
    }

    &.heightMode{
      margin: auto;
      height: 100%;
      display: inline-block;
      img{
        height:100%;
      }
    }

  }
  .title{
    font-size:30px;
    padding: 1rem;
    font-weight: bold;
    text-align: center;
  }


  .circle{
    position: absolute;
    width: 8%;
    height: 15%;
    border-radius: 50%;

    &.circle-red{
      background: radial-gradient(ellipse at center, #ffffff29 29%, #f95a5a 100%);
    }
    &.circle-blue{
      background: radial-gradient(ellipse at center, #ffffff29 29%, #6393c1 100%);
    }
    &.circle-green{
      //    background-color: rgba(41, 197, 31, 0.2901960784313726);
      background: radial-gradient(ellipse at center, #ffffff29 29%, #2f940d 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
    }


    .miniFont{
      font-size: 15px;
    }
    .temp{
      // position: absolute;
      // top: 40%;
      // left:50%;
      transform: translate(-50%,-50%);
      color: white;
      font-weight: bold;
      font-size: 1em;
    }
    .humid{
      // position: absolute;
      // top: 60%;
      // left:50%;
      transform: translate(-50%,-50%);
      color: white;
      font-weight: bold;
      font-size: 1em;
    }
    &.position0{
      display:none;
    }
    &.position1{
      left: 12%;
      bottom: 30%;
    }
    &.position2{
      left: 19%;
      bottom: 45%;
    }
    &.position3{
      left: 28%;
      top: 30%;
    }
    &.position4{
      left: 30%;
      top: 15%;
    }
    &.position5{
      left: 40%;
      top: 20%;
    }
    &.position6{
      left: 50%;
      top: 30%;
    }
    &.position7{
      left: 59%;
      top: 35%;
    }
    &.position8{
      left: 67%;
      top: 40%;
    }
    &.position9{
      right: 16%;
      top: 46%;
    }
    &.position10{
      left: 44%;
      top: 46%;
    }
    &.position11{
      .textDiv{
        height: 25%;
        width: 50%;
        background-color: rgba(0, 0, 0, 0.1);
        border-radius: 50%;
        margin: auto;
        margin-top: 55%;
        transform: translate(0%,-50%);
      }
      left: 33%;
      bottom: 27%;
    }
    &.position12{
      .textDiv{
        height: 25%;
        width: 50%;
        background-color: rgba(0, 0, 0, 0.1);
        border-radius: 50%;
        margin: auto;
        margin-top: 55%;
        transform: translate(0%,-50%);
      }
      left: 42%;
      bottom: 23%;
    }
    &.position13{
      .textDiv{
        height: 25%;
        width: 50%;
        background-color: rgba(0, 0, 0, 0.1);
        border-radius: 50%;
        margin: auto;
        margin-top: 55%;
        transform: translate(0%,-50%);
      }
      right: 29%;
      top: 55%;
    }
    &.position14{
      right: 33%;
      bottom: 16%;
    }
    &.position15{
      right: 22.5%;
      bottom: 10.5%;
    }

  }


</style>
