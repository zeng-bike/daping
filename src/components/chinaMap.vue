
<template>
  <div>
    <div id="regionCharts" :style="{width: '100%', height: '400px'}"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dataList: [
        {
          name: "南海诸岛",
          value: 10
        },
        {
          name: "北京",
          value: 54
        },
        {
          name: "天津",
          value: 13
        },
        {
          name: "上海",
          value: 40
        },
        {
          name: "重庆",
          value: 75
        },
        {
          name: "河北",
          value: 13
        },
        {
          name: "河南",
          value: 83
        },
        {
          name: "云南",
          value: 11
        },
        {
          name: "辽宁",
          value: 2
        },
        {
          name: "黑龙江",
          value: 15
        },
        {
          name: "湖南",
          value: 69
        },
        {
          name: "安徽",
          value: 6
        },
        {
          name: "山东",
          value: 4
        },
        {
          name: "新疆",
          value: 4
        },
        {
          name: "江苏",
          value: 3
        },
        {
          name: "浙江",
          value: 10
        },
        {
          name: "江西",
          value: 4
        },
        {
          name: "湖北",
          value: 5
        },
        {
          name: "广西",
          value: 3
        },
        {
          name: "甘肃",
          value: 7
        },
        {
          name: "山西",
          value: 5
        },
        {
          name: "内蒙古",
          value: 7
        },
        {
          name: "陕西",
          value: 22
        },
        {
          name: "吉林",
          value: 4
        },
        {
          name: "福建",
          value: 18
        },
        {
          name: "贵州",
          value: 5
        },
        {
          name: "广东",
          value: 10
        },
        {
          name: "青海",
          value: 1
        },
        {
          name: "西藏",
          value: 0
        },
        {
          name: "四川",
          value: 44
        },
        {
          name: "宁夏",
          value: 4
        },
        {
          name: "海南",
          value: 22
        },
        {
          name: "台湾",
          value: 3
        },
        {
          name: "香港",
          value: 5
        },
        {
          name: "澳门",
          value: 5
        }
      ],
      myChart: null,
      swpTime: null
    };
  },

  created() {},
  mounted() {
    this.drawregionCharts();
  },
  methods: {
    swp() {
      let that = this;
      let i = 0;
      let dataIndex;
      const compare = function(property) {
        return function(a, b) {
          return b[property] - a[property];
        };
      };
      const sortData = that.dataList.concat().sort(compare("value"));
      that.swpTime = setInterval(() => {
        if (i >= that.dataList.length) {
          i = 0;
        }
    that.myChart.dispatchAction({
                type: 'downplay',
                seriesIndex: 0,
                   dataIndex: dataIndex
            })
        let name = sortData[i].name;
        dataIndex = that.dataList.findIndex(item => {
          return item.name === name;
        });
        i++;
       
        that.myChart.dispatchAction({
          type: "highlight",
          // 可选，系列 index，可以是一个数组指定多个系列
          seriesIndex: 0,
          // 可选，系列名称，可以是一个数组指定多个系列
          // seriesName: string|Array,
          // 数据的 index，如果不指定也可以通过 name 属性根据名称指定数据
          dataIndex: dataIndex
          // 可选，数据名称，在有 dataIndex 的时候忽略
          //name: '河北'
        });
        that.myChart.dispatchAction({
          type: "showTip",
          seriesIndex: 0,
          dataIndex: dataIndex
        });
      }, 1000);
    },
    drawregionCharts() {
      // let dataList = [
      //   { name: "北京", value: 200 },
      //   { name: "四川", value: 800 }
      // ]; // 该数据是从服务器获取到的数据
      let option = {
        title: {
          text: "123",
          left: "center"
        },
        tooltip: {
          //backgroundColor:'transparent',
          // trigger: "item",

          formatter: function(params) {
            return params.name; //自行定义formatter格式
          }
          //textStyle:{color:'#fff'},
        },
        visualMap: [
          {
            type: "piecewise",
            pieces: [
              {
                min: 1,
                max: 5,
                color: "#c6d9f1"
              },
              {
                min: 6,
                max: 15,
                color: "#8eb4e3"
              },
              {
                min: 16,
                max: 25,
                color: "#40a9ed"
              },
              {
                min: 26,
                max: 36,
                color: "#4B93C5"
              },
              { min: 37, color: "#0E2061" }
            ],
            textStyle: {
              color: "#fff"
            },
            bottom: 10
          }
        ],
        geo: {
          map: "china",
          show: true,
          zoom: 1.2, //视角缩放比例

          itemStyle: {
            areaColor: "rgba(23,55,94,0.5)",
            borderColor: "#f2f3f5"
          }
        },
        series: [
          {
            type: "map",
            map: "china",
            // geoIndex: 0,
            zoom: 1.2,
            data: this.dataList,
            label: {
              show: true,
              color: "#fff",
              fontSize: 12
            },
            emphasis: {
              label:{
                show:true,
                color:'#fff',
                fontWeight:'bold',
                textShadowColor:'#29c7ff',
                textShadowBlur:20
              },
              itemStyle: {
                areaColor: "#4473bb"
              }
            },
            itemStyle: {
              areaColor: "rgba(23,55,94,0.5)",
              borderColor: "f2f3f5"
            }
          }
        ]
      };
      this.myChart = this.$echarts.init(
        document.getElementById("regionCharts")
      );
      this.myChart.setOption(option);
      this.swp();
    //   this.$nextTick(() => {
    //     this.myChart.resize(); // 这里是为了解决，tab刷新的时候，图表不刷新的问题。
    //   });
    }
  }
};
</script>

<style scoped>
#china_map_box {
  height: 100%;
  position: relative;
}
#china_map_box #china_map {
  height: 100%;
}
#china_map_box .china_map_logo {
  position: absolute;
  top: 0;
  left: 0;
  width: 45px;
}
#china_map .tooltip_style {
  line-height: 1.7;
  overflow: hidden;
}
#china_map .tooltip_left {
  float: left;
}
#china_map .tooltip_right {
  float: right;
}
</style>

