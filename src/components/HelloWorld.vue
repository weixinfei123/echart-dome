<template>
  <div class="hello">
    <div id="map"></div>
  </div>
</template>

<script>
let echarts = require("echarts");
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  // created() {
  // },
  mounted() {
    this.$nextTick(() => {
      this.getMapData();
    });
  },
  methods: {
    getMapData() {
      let myEchart = echarts.init(document.getElementById("map"));
      var _this = this;
      var geoCoordMap = [
        {
          name: "荔湾区",
          value: [113.243038, 23.124943],
          label: "荔湾区-1号店"
        },
        {
          name: "海珠区",
          value: [113.262008, 23.103131],
          label: "海珠区-1号店"
        },
        {
          name: "越秀区",
          value: [113.280714, 23.125624],
          label: "越秀区-1号店"
        },
        {
          name: "天河区",
          value: [113.335367, 23.13559],
          label: "天河区-1号店"
        },
        {
          name: "黄埔区",
          value: [113.450761, 23.103239],
          label: "黄埔区-1号店"
        },
        {
          name: "白云区",
          value: [113.262831, 23.162281],
          label: "白云区-1号店"
        },
        {
          name: "番禺区",
          value: [113.364619, 22.938582],
          label: "番禺区-1号店"
        },
        {
          name: "南沙区",
          value: [113.53738, 22.794531],
          label: "南沙区-1号店"
        },
        {
          name: "从化区",
          value: [113.587386, 23.545283],
          label: "从化区-1号店"
        },
        {
          name: "花都区",
          value: [113.211184, 23.39205],
          label: "花都区-1号店"
        },
        {
          name: "增城区",
          value: [113.829579, 23.290497],
          label: "增城区-1号店"
        }
      ];
      var rawData = [
        { name: "荔湾区", value: [10, 20, 30] },
        { name: "海珠区", value: [10, 20, 30] },
        { name: "越秀区", value: [10, 20, 30] },
        { name: "天河区", value: [10, 20, 30] },
        { name: "黄埔区", value: [10, 20, 30] },
        { name: "白云区", value: [10, 20, 30] },
        { name: "番禺区", value: [10, 20, 30] },
        { name: "南沙区", value: [10, 20, 30] },
        { name: "从化区", value: [10, 20, 30] },
        { name: "花都区", value: [10, 20, 30] },
        { name: "增城区", value: [10, 20, 30] }
      ];

      let options = {
        backgroundColor: "rgba(0,0,0,0.2)",
        title: {
          text: "广州市 echart -门店分布图",
          subtext: "数据来源于我瞎编的",
          sublink: "http://lengff.xyz",
          x: "center",
          itemGap: 2,
          subtextStyle: {
            color: "#fff"
          },
          textStyle: {
            color: "#fff"
          }
        },
        tooltip: {
          trigger: "item",
          textStyle: {
            color: "hotpink"
          },
          formatter: function(params) {
            return params.name + " : " + params.data.label;
          }
        },
        legend: {
          orient: "vertical",
          y: "bottom",
          x: "right",
          data: ["测试门店分布"],
          textStyle: {
            color: "#fff"
          }
        },
        geo: {
          map: "guangzhou",
          roam: true,
          zoom: 1.2,
          layoutSize: "50%",
          label: {
            emphasis: {
              show: true
            }
          },
          itemStyle: {
            normal: {
              areaColor: "#2b87bb",
              borderColor: "#73ffb3"
            },
            emphasis: {
              borderColor: "#fff",
              borderWidth: 1,
              areaColor: "blue"
            }
          }
        },
        toolbox: {
          show: true,
          right: "left",
          top: 0,
          feature: {
            dataView: { readOnly: false },
            restore: {},
            saveAsImage: {}
          }
        },
        series: [
          {
            name: "测试门店分布",
            type: "scatter",
            coordinateSystem: "geo",
            label: {
              normal: {
                color: "#fff",
                formatter: "{b}",
                position: "bottom",
                show: true
              }
            },
            data: geoCoordMap,
            symbolSize: 30,
            encode: { value: 2 },
            symbol: "pin",
            zlevel: 1
          }
        ]
      };

      //给每个城市对应的坐标处加上柱状图
      function renderEachCity() {
        var option = {
          xAxis: [],
          yAxis: [],
          grid: [],
          series: []
        };
        _this.$nextTick(() => {
          echarts.util.each(rawData, function(dataItem, idx) {
            // console.log(dataItem,idx);      //["南京", 10, 20, 30], 0
            // console.log(dataItem[0]);       //"南京"Item[0]]; //获得城市的坐标
            var inflationData = [
              dataItem.value[0],
              dataItem.value[1],
              dataItem.value[2]
            ];
            var geoCoord;
            var coord;
            for (var i = 0; i < geoCoordMap.length; i++) {
              if (dataItem.name == geoCoordMap[i].name) {
                geoCoord = geoCoordMap[i].value;
              }
            }
            coord = myEchart.convertToPixel("geo", geoCoord); //转换坐标系上的点到像素坐标值。
            console.log(coord);
            idx += "";
            option.xAxis.push({
              id: idx,
              gridId: idx,
              type: "category",
              name: dataItem.name,
              nameTextStyle: {
                color: "#F1E04F"
              },
              nameLocation: "middle",
              nameGap: 3,
              splitLine: {
                show: false
              },
              axisTick: {
                show: false
              },
              axisLabel: {
                show: false
              },
              axisLine: {
                show: false
              },
              minInterval: 10,
              data: ["学校", "教师", "学生"],
              z: 100
            });
            option.yAxis.push({
              id: idx,
              gridId: idx,
              splitLine: {
                show: false
              },
              axisTick: {
                show: false
              },
              axisLabel: {
                show: false
              },
              axisLine: {
                show: false,
                lineStyle: {
                  color: "#1C70B6"
                }
              },
              max: 50,
              z: 100000
            });

            option.grid.push({
              id: idx,
              width: 30,
              height: 40,
              left: coord[0] - 15,
              top: coord[1] - 15,
              z: 100000
            });
            options.series.push({
              id: idx,
              type: "bar",
              xAxisId: idx,
              yAxisId: idx,
              barWidth: 7,
              barGap: 1,
              barCategoryGap: 0,
              data: inflationData,
              z: 1000,
              itemStyle: {
                normal: {
                  color: function(params) {
                    // 柱状图每根柱子颜色
                    var colorList = ["red", "red", "red"];
                    return colorList[params.dataIndex];
                  }
                }
              }
            });
          });
          let newOpt = Object.assign({}, option, options);
          myEchart.setOption(newOpt);
        });
      }
      this.$http.get("/user/mapdata").then(res => {
        // 注册地图
        echarts.registerMap("guangzhou", res.data);
        myEchart.setOption(options);
        this.$nextTick(() => {
          renderEachCity();
        });
        // ;
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#map {
  width: 800px;
  height: 800px;
  margin: 0 auto;
}
</style>
