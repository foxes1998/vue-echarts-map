<template>
  <div class="page">
    <div class="header">
      <div class="title">
        2019年度土地交易信息总览
      </div>
      <div class="title-bottom">
        <div class="bottom-sidebar"></div>
      </div>
    </div>

    <div class="row">
      <div class="col-md-4">
        <div class="col-sm-12 col-md-12">
          <div class="box-content">
            <div class="statistic-box row">
              <div class="statistic-item col-md-6" v-for="item in indexData" v-bind:key="item.name">
                <div class="icon-content">
                  <span class="iconfont icon-shuju"></span>
                </div>
                <div class="text-content">
                  <div class="text">
                    <span class="counter">
                      <ICountUp :delay="delay" :endVal="item.value" :options="options" />
                    </span>
                  </div>
                  <div class="sub-title">{{item.name}}</div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- <h6 class="pie-title">订单总金额饼图</h6> -->
        <div ref="multiPieChart" style="height: 500px" class="chart"></div>

        <div class="col-md-12">
          <table class="table border-table darkblue" style="width: 100%" cellpadding="0" cellspacing="0">
            <tr>
              <td class="th" style="width: 90px;">排名</td>
              <td class="th">区域</td>
              <td class="th td_right">
                年订单量
              </td>
              <td class="th td_right">
                月订单量
              </td>
            </tr>
            <tr v-for="(item,index) in datalist" :key="item.id">
              <td class="border_bottom border_left_no">{{index+1}}</td>
              <td class="border_bottom enable" @click='tdSelect(item)'>
                {{item.area}}
              </td>
              <td class="td_right">
                {{item.nValue}}
              </td>
              <td class="td_right">
                {{item.yValue}}
              </td>
            </tr>
          </table>
        </div>

      </div>

      <div class="col-md-8">
        <div class="row">
          <div class="col-sm-12 col-md-12">

            <div class="box-content">
              <div class="caption">
                <div class="main">
                  <h2>
                    <ICountUp :endVal="totalSales" />
                    <small class="number warning">80.99%</small></h2>
                </div>
                <div>
                  <h6>年交易总额(万)(vs去年交易总额)</h6>
                </div>
              </div>
              <div style="height:400px;" id="mapContainer"></div>
              <div ref="categoryChart" style="height: 400px" class="chart"></div>
            </div>
          </div>
        </div>
      </div>

    </div>
    <div class="footer">
      <div class="bottom-line"></div>

    </div>
  </div>
</template>
<script>
  import ICountUp from 'vue-countup-v2'
  import 'echarts/map/js/china.js'
  import chinaJson from 'echarts/map/json/china.json'

  /** 将数值转换为万单位的数值 */
  function formatLargeNumber(num) {
    num = num / 10000;
    num = num.toFixed(2);
    return num;
  }

  var textColor = "9AA8D4";
  var monthArray = ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月']

  export default {
    name: 'index',
    components: {
      ICountUp
    },

    data() {
      return {
        delay: 0,
        options: {
          useEasing: true,
          useGrouping: true,
          separator: ',',
          decimal: '.',
          prefix: '',
          suffix: ''
        },
        indexData: [{
          name: '12月成交额(万元)',
          value: parseFloat(formatLargeNumber(774892333.00))
        }, {
          name: '对比上月增长(万元)',
          value: parseFloat(formatLargeNumber(320892931.00))
        }, {
          name: '12月成交订单数',
          value: 11448
        }, {
          name: '年成交订单数',
          value: 34065
        }, {
          name: '年交易面积(万平方米)',
          value: parseFloat(formatLargeNumber(1956317224.31))
        }, {
          name: '12月交易面积(万平方米)',
          value: parseFloat(formatLargeNumber(569400859.00))
        }],
        totalSales: 2299921824.84,
        datalist: [{
            ranking: 1,
            area: "浙江",
            nValue: 248612501,
            yValue: 132647947
          },
          {
            ranking: 1,
            area: "福建",
            nValue: 168330491,
            yValue: 71097654
          },
          {
            ranking: 1,
            area: "江苏",
            nValue: 160013979,
            yValue: 20518170
          }
        ],
        category: [{
            "categoryid": "1",
            "category": "住宅用地"
          },
          {
            "categoryid": "2",
            "category": "工业用地"
          },
          {
            "categoryid": "3",
            "category": "商业/办公用地"
          },
          {
            "categoryid": "4",
            "category": "综合用地"
          },
          {
            "categoryid": "5",
            "category": "其他用地"
          }
        ],

        monthCategoryData: {
          "住宅用地": [1861948, 913927, 22455374, 668794, 2019212, 1723540, 103971,
            49293, 23297914, 893745, 807279, 125088572
          ],
          "工业用地": [302915, 187430, 141258, 359227, 236175, 315220, 62148, 421689,
            1147284, 1312205, 1427241, 4527137
          ],
          "商业/办公用地": [943227, 165619, 337946, 353902, 607271, 10810111, 0, 0, 0,
            12789, 0, 64200],
          "综合用地": [68945135, 2556749, 3539189, 21966316, 2899706, 10574287, 0, 0,
            0, 36186, 14100, 87200],
          "其他用地": [3613416, 9468921, 896129, 905084, 6426396, 149322714, 88775962,
            254250336, 250653267, 124496149, 451750782, 645125224
          ]
        },
        categoryClassifyData: [{
          "categoryid": "1",
          "categoryName": "省份",
          "data": [{
              "name": "广东",
              "value": 117338831
            },
            {
              "name": "湖南",
              "value": 128465823
            },
            {
              "name": "广西",
              "value": 139718305
            },
            {
              "name": "四川",
              "value": 144486461
            },
            {
              "name": "河北",
              "value": 154738846
            },
            {
              "name": "内蒙古",
              "value": 155764495
            },
            {
              "name": "江苏",
              "value": 160013979
            },
            {
              "name": "福建",
              "value": 168330491
            },
            {
              "name": "浙江",
              "value": 248612501
            },
            {
              "name": "其他省份省略",
              "value": 0
            }
          ]
        }, {
          "categoryid": "2",
          "categoryName": "时间",
          "data": [{
              "name": "1月",
              "value": 75666643
            },
            {
              "name": "2月",
              "value": 13292648
            },
            {
              "name": "3月",
              "value": 27369898
            },
            {
              "name": "4月",
              "value": 24253324
            },
            {
              "name": "5月",
              "value": 12188761
            },
            {
              "name": "6月",
              "value": 172745875
            },
            {
              "name": "7月",
              "value": 88942081
            },
            {
              "name": "8月",
              "value": 254721318
            },
            {
              "name": "9月",
              "value": 275098465
            },
            {
              "name": "10月",
              "value": 126751074
            },
            {
              "name": "11月",
              "value": 453999402
            },
            {
              "name": "12月",
              "value": 774892333
            }
          ]
        }, {
          "categoryid": "3",
          "categoryName": "用途",
          "data": [{
              "name": "综合用地",
              "value": 938
            },
            {
              "name": "其他用地",
              "value": 22378
            },
            {
              "name": "商业/办公用地",
              "value": 328
            },
            {
              "name": "工业用地",
              "value": 9439
            },
            {
              "name": "住宅用地",
              "value": 982
            }
          ]
        }, {
          "categoryid": "4",
          "categoryName": "面积",
          "data": [{
              "name": "小于等于200平方米",
              "value": 848
            },
            {
              "name": "200到1千平方米",
              "value": 1203
            },
            {
              "name": "1千到1万平方米",
              "value": 7158
            },
            {
              "name": "1万到10万平方米",
              "value": 18301
            },
            {
              "name": "大于10万平方米",
              "value": 6555
            }
          ]
        }],
        provinceSale: [{
            "name": "海南",
            "value": 428
          },
          {
            "name": "黑龙江",
            "value": 454
          },
          {
            "name": "重庆",
            "value": 547
          },
          {
            "name": "陕西",
            "value": 840
          },
          {
            "name": "福建",
            "value": 863
          },
          {
            "name": "内蒙古",
            "value": 1090
          },
          {
            "name": "河南",
            "value": 1114
          },
          {
            "name": "广东",
            "value": 1147
          },
          {
            "name": "贵州",
            "value": 1204
          },
          {
            "name": "吉林",
            "value": 1209
          },
          {
            "name": "江西",
            "value": 1382
          },
          {
            "name": "湖北",
            "value": 1453
          },
          {
            "name": "江苏",
            "value": 2348
          },
          {
            "name": "山东",
            "value": 2552
          },
          {
            "name": "山西",
            "value": 640
          },
          {
            "name": "甘肃",
            "value": 818
          },
          {
            "name": "河北",
            "value": 3058
          },
          {
            "name": "四川",
            "value": 1840
          },
          {
            "name": "安徽",
            "value": 2139
          },
          {
            "name": "浙江",
            "value": 2311
          },
          {
            "name": "西藏",
            "value": 3
          },
          {
            "name": "云南",
            "value": 1508
          },
          {
            "name": "湖南",
            "value": 1527
          },
          {
            "name": "广西",
            "value": 1730
          },
          {
            "name": "北京",
            "value": 55
          },
          {
            "name": "上海",
            "value": 162
          },
          {
            "name": "天津",
            "value": 186
          },
          {
            "name": "宁夏",
            "value": 300
          },
          {
            "name": "青海",
            "value": 351
          },
          {
            "name": "辽宁",
            "value": 398
          },
          {
            "name": "新疆",
            "value": 408
          },
        ],
        provinceSaleMoney: [{
            "name": "天津",
            "value": 5884259
          },
          {
            "name": "广东",
            "value": 117338831
          },
          {
            "name": "辽宁",
            "value": 4523308
          },
          {
            "name": "云南",
            "value": 31537300
          },
          {
            "name": "湖北",
            "value": 107847684
          },
          {
            "name": "四川",
            "value": 144486461
          },
          {
            "name": "西藏",
            "value": 18906
          },
          {
            "name": "宁夏",
            "value": 17294325
          },
          {
            "name": "海南",
            "value": 83234327
          },
          {
            "name": "贵州",
            "value": 73539689
          },
          {
            "name": "江苏",
            "value": 160013979
          },
          {
            "name": "湖南",
            "value": 128465823
          },
          {
            "name": "重庆",
            "value": 7091941
          },
          {
            "name": "山东",
            "value": 64055556
          },
          {
            "name": "北京",
            "value": 11525475
          },
          {
            "name": "新疆",
            "value": 2363107
          },
          {
            "name": "黑龙江",
            "value": 100559140
          },
          {
            "name": "浙江",
            "value": 248612501
          },
          {
            "name": "江西",
            "value": 40208537
          },
          {
            "name": "河南",
            "value": 8705829
          },
          {
            "name": "广西",
            "value": 139718305
          },
          {
            "name": "青海",
            "value": 36227792
          },
          {
            "name": "山西",
            "value": 15314526
          },
          {
            "name": "福建",
            "value": 168330491
          },
          {
            "name": "河北",
            "value": 154738846
          },
          {
            "name": "上海",
            "value": 11691091
          },
          {
            "name": "甘肃",
            "value": 42334586
          },
          {
            "name": "内蒙古",
            "value": 155764495
          },
          {
            "name": "吉林",
            "value": 86883662
          },
          {
            "name": "安徽",
            "value": 105576726
          },
          {
            "name": "陕西",
            "value": 26034321
          }
        ]
      }
    },
    methods: {
      drawMultiChart2() {
        var that = this;
        var categorySale = this.categorySale;

        var provinceSale = JSON.parse(JSON.stringify(this.provinceSale));

        var geoCoordMap = {
          '上海': [121.4648, 31.2891],
          '新疆': [87.9236, 43.5883],
          '甘肃': [103.5901, 36.3043],
          '北京': [116.4551, 40.2539],
          '江苏': [118.8062, 31.9208],
          '广西': [108.479, 23.1152],
          '江西': [116.0046, 28.6633],
          '安徽': [117.29, 32.0581],
          '内蒙古': [111.4124, 40.4901],
          '黑龙江': [127.9688, 45.368],
          '天津': [117.4219, 39.4189],
          '山西': [112.3352, 37.9413],
          '广东': [113.5107, 23.2196],
          '四川': [103.9526, 30.7617],
          '西藏': [91.1865, 30.1465],
          '云南': [102.9199, 25.4663],
          '浙江': [119.5313, 29.8773],
          '湖北': [114.3896, 30.6628],
          '辽宁': [123.1238, 42.1216],
          '山东': [117.1582, 36.8701],
          '海南': [110.3893, 19.8516],
          '河北': [114.4995, 38.1006],
          '福建': [119.4543, 25.9222],
          '青海': [101.4038, 36.8207],
          '陕西': [109.1162, 34.2004],
          '贵州': [106.6992, 26.7682],
          '河南': [113.4668, 34.6234],
          '重庆': [107.7539, 30.1904],
          '宁夏': [106.3586, 38.1775],
          '吉林': [125.8154, 44.2584],
          '湖南': [113.0823, 28.2568]
        }
        var convertData2 = function(originData) {
          var res = [];
          var data = JSON.parse(JSON.stringify(originData))
          for (var i = 0; i < data.length; i++) {
            var geoCoord = geoCoordMap[data[i].name];
            if (geoCoord) {
              res.push({
                name: data[i].name,
                value: geoCoord.concat(parseInt((data[i].value)))
              });
            }
          }
          return res;
        };
        this.$echarts.registerMap('china', chinaJson)
        var myChart = this.$echarts.init(document.getElementById('mapContainer'));
        //每次窗口大小改变的时候都会触发onresize事件，这个时候我们将echarts对象的尺寸赋值给窗口的大小这个属性，从而实现图表对象与窗口对象的尺寸一致的情况
        window.onresize = myChart.resize;
        var option = {
          backgroundColor: 'transparent',
          title: {
            top: 20,
            left: 100,
            text: '订单地域分布',
            subtext: '',
            x: 'left',
            textStyle: {
              color: '#ccc'
            }
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: { // 坐标轴指示器，坐标轴触发有效
              type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
            },
            formatter: function(params) {
              if (typeof(params.value)[2] == "undefined") {
                //return params.name + ' : ' + params.value;
              } else { //只有数据不为空才显示
                return params.name + ' : ' + params.value[2];
              }
            }
          },
          visualMap: { //左侧小导航图标
            show: true,
            x: 'left',
            y: 'center',
            top: '250',
            seriesIndex: [0, 1],
            textStyle: {
              color: "#8fc8f2"
            },
            splitList: [{
                start: 2200
              }, {
                start: 1000,
                end: 2200
              },
              {
                start: 500,
                end: 1000
              }, {
                start: 100,
                end: 500
              },
              {
                start: 0,
                end: 100
              }
            ],
            color: ['#6470ea', '#F4E925', '#85daef', '#74e2ca', '#e6ac53']
          },
          //布局
          grid: [{
            x: '55%',
            y: '5%',
            width: '40%',
            height: '90%'
          }, ],
          xAxis: [{
            gridIndex: 0,
            type: 'value',
            axisLabel: {
              show: true,
            },
          }],
          yAxis: [{
            gridIndex: 0,
            type: 'category',

            axisLabel: {
              show: true,
              textStyle: {
                color: '#fff'
              }
            },
            label: {
              normal: {
                show: true,
              }
            },
            data: (function getXYData() {
              var data = provinceSale;
              var property = "name";
              var res = [];
              data.forEach(function(item) {
                res.push(item[property])
              })
              return res
            })(),
          }],
          tooltip: {
            trigger: 'item'
          },
          //布局e
          geo: {
            show: true,
            map: 'china',
            label: {
              normal: {
                show: true,
                textStyle: {
                  color: '#ccc'
                }
              },
              emphasis: {
                show: false,
              }
            },
            roam: true, //是否开启鼠标缩放和平移漫游
            itemStyle: {
              normal: {
                areaColor: 'transparent',
                borderWidth: 1,
                shadowColor: 'rgba(63, 218, 255, 0.5)',
                shadowBlur: 30
              },
              emphasis: {
                areaColor: '#2B91B7',
                color: '#fff'
              }
            }
          },
          //调整显示级别
          layoutCenter: ['25%', '50%'],
          layoutSize: 400,
          series: [{
              name: '订单数',
              type: 'scatter',
              coordinateSystem: 'geo',
              data: convertData2(provinceSale),
              symbolSize: function(val) {
                return val[2] / 100;
              },
              label: {
                normal: {
                  formatter: function(a) {
                    return a.value[2] + "单";
                  },
                  position: 'right',
                  show: false
                },
                emphasis: {
                  show: true
                }
              },
              tooltip: {
                formatter: function(a) {
                  return `${a.name} ${a.seriesName}<br/>${a.value[2]}单`
                }
              },
            },
            {
              name: '订单数前三名',
              type: 'effectScatter',
              coordinateSystem: 'geo',
              data: convertData2(provinceSale.sort(function(a, b) {
                return b.value - a.value;
              }).slice(0, 3)),
              symbolSize: function(val) {
                return val[2] / 100;
              },
              showEffectOn: 'render',
              rippleEffect: {
                brushType: 'stroke'
              },
              hoverAnimation: true,
              label: {
                normal: {
                  formatter: function(a) {
                    return a.value[2] + "单";
                  },
                  position: 'right',
                  show: true
                }
              },
              tooltip: {
                formatter: function(a) {
                  return `${a.name} ${a.seriesName}<br/>${a.value[2]}单`
                }
              },
              zlevel: 1
            },
            {
              id: 'bar',
              name: '年订单总额按省份',
              stack: 'bar',
              type: 'bar',
              xAxisIndex: 0,
              yAxisIndex: 0,
              tooltip: {
                formatter: function(a, b, c) {
                  return `${a.seriesName}<br/>${a.name}<br/>${formatLargeNumber(a.value)}万元`;
                }
              },
              itemStyle: {
                color: '#cc6907'
              },
              markPoint: {
                data: [{
                    type: 'max',
                    name: '最大值'
                  },
                  {
                    type: 'min',
                    name: '最小值'
                  }
                ]
              },
              markLine: {
                data: [{
                  type: 'average',
                  name: '平均值'
                }]
              },
              z: 3,
              data: that.provinceSaleMoney
            },
          ]
        };
        myChart.setOption(option);
      },

      drawMultiCategoryPieChart() {
        var categoryClassifyData = this.categoryClassifyData;
        var option = {
          legend: {
            show: false
          },
          color: ['#3682be', '#45a776', '#f05326', '#eed777', '#334f65', '#b3974e',
            '#38cb7d', '#ddae33', '#844bb3', '#93c555', '#5f6694', '#df3881'
          ],
          tooltip: {
            formatter: function(a, b, c) {
              if (a.seriesIndex < 2) {
                return `${a.name}  ${a.seriesName}<br/>${formatLargeNumber(a.value)}万元<br/>占比: ${a.percent}%`;
              } else if (a.seriesIndex === 3) {
                return `${a.name}  ${a.seriesName}<br/>${a.value}单<br/>占比: ${a.percent}%`;
              } else {
                return `面积在 ${a.name}  ${a.seriesName}<br/>${a.value}单<br/>占比: ${a.percent}%`;
              }

            }
          },
          series: [{
            type: 'pie',
            name: '省交易额',
            radius: 50,
            center: ['25%', '20%'],
            data: categoryClassifyData[0].data
          }, {
            type: 'pie',
            name: '交易额',
            radius: 50,
            center: ['65%', '20%'],
            data: categoryClassifyData[1].data
          }, {
            type: 'pie',
            name: '订单数占比',
            radius: 50,
            center: ['25%', '65%'],
            data: categoryClassifyData[2].data
          }, {
            type: 'pie',
            name: '订单数',
            radius: 50,
            center: ['65%', '65%'],
            data: categoryClassifyData[3].data
          }]
        };
        let myChart = this.$echarts.init(this.$refs.multiPieChart);
        window.onresize = myChart.resize;
        myChart.setOption(option)
      },

      drawCategorybyMonth() {
        var category = this.category;
        var itemStyle = {
          normal: {
            label: {
              formatter: function(a) {
                return formatLargeNumber(a.value);
              }
            }
          },
        };
        var markPoint = {
          data: [{
            type: 'max',
            name: '最大值'
          }, ],
          itemStyle: itemStyle
        };
        var monthCategoryData = this.monthCategoryData;
        var option = {
          title: {
            text: '月交易总额（按 住宅用地）(万)',
            textStyle: {
              color: '#9AA8D4'
            }
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: { // 坐标轴指示器，坐标轴触发有效
              type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
            },

            formatter: function(a, b, c) {

              return `${a[0].axisValue}<br/>${a[0].seriesName}  ${formatLargeNumber(a[0].value)}万元`;
            }
          },
          legend: {
            left: 'right',
            data: (function getXYData(data, property) {
              var data = category;
              var property = "category";
              var res = [];
              data.forEach(function(item) {
                res.push(item[property])
              })

              return res
            })(),
            textStyle: {
              color: textColor
            },
            selectedMode: 'single'
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
          },
          yAxis: {
            type: 'value',
            axisLabel: {
              textStyle: {
                color: '#fff'
              },
            }
          },
          xAxis: {
            type: 'category',
            data: monthArray,
            axisLabel: {
              textStyle: {
                color: '#fff'
              }
            }
          },
          series: [{
              name: "住宅用地",
              type: 'bar',
              stack: '总量',
              label: {
                normal: {
                  show: true,
                  position: 'insideRight'
                }
              },
              data: monthCategoryData["住宅用地"],
              itemStyle,
              markPoint,
            },
            {
              name: "工业用地",
              type: 'bar',
              stack: '总量',
              label: {
                normal: {
                  show: true,
                  position: 'insideRight'
                }
              },
              data: monthCategoryData["工业用地"],
              itemStyle,
              markPoint
            },
            {
              name: "商业/办公用地",
              type: 'bar',
              stack: '总量',
              label: {
                normal: {
                  show: true,
                  position: 'insideRight'
                }
              },
              data: monthCategoryData["商业/办公用地"],
              itemStyle,
              markPoint
            },
            {
              name: "综合用地",
              type: 'bar',
              stack: '总量',
              label: {
                normal: {
                  show: true,
                  position: 'insideRight'
                }
              },
              data: monthCategoryData["综合用地"],
              itemStyle,
              markPoint
            },
            {
              name: "其他用地",
              type: 'bar',
              stack: '总量',
              label: {
                normal: {
                  show: true,
                  position: 'insideRight'
                }
              },
              data: monthCategoryData["其他用地"],
              itemStyle,
              markPoint,
            }
          ]
        };
        this.myChart = this.$echarts.init(this.$refs.categoryChart);
        window.onresize = this.myChart.resize;
        this.myChart.setOption(option)

        const data = this.myChart.getOption().legend[0].data;

        let i = 0 // 首次总是从0开始的
        // 开始轮播
        this.timer = setInterval(() => {
          //legendUnSelect（取消选中图例）
          this.myChart.dispatchAction({
            type: 'legendUnSelect',
            name: data[i % data.length]
          })
          // legendToggleSelect（切换图例的选中状态）
          this.myChart.dispatchAction({
            type: 'legendToggleSelect',
            name: data[++i % data.length]
          })
          option.title.text = `月交易总额（按 ${data[ i % data.length ]}）`; //动态设置标题
          this.myChart.setOption(option)
        }, 3500)
      },

    },
    mounted() {
      this.drawMultiChart2();
      this.drawMultiCategoryPieChart();
      this.drawCategorybyMonth();
    }
  }
</script>
<style scoped>
  .header {
    margin: 4px;
  }

  .header .title {
    font-size: 24px;
    color: #fff;

  }

  .title-bottom {
    height: 30px;
    justify-content: center;
    display: flex;

  }

  .bottom-sidebar {
    width: 100%;
    height: 100%;
    opacity: 1;
    background-image: url("~@/assets/images/bottombar.gif");
    background-size: 100%;
    background-repeat: no-repeat;
    background-position: center center;
    width: 400px;
  }

  .bottom-line {
    width: 100%;
    height: 30px;
    background-image: url("~@/assets/images/bottomline.png");
    background-size: 100%;
    background-repeat: no-repeat;
    background-position: center center;
  }

  .box-content {
    display: block;
    line-height: 1.42857143;
    -webkit-transition: border 0.2s ease-in-out;
    -o-transition: border 0.2s ease-in-out;
    transition: border 0.2s ease-in-out;
  }

  .box-content.shadow {
    box-shadow: 1px 2px 8px 0px #888;

  }

  .statistic-item {
    display: flex;
    margin-bottom: 10px;
    align-items: center;
    overflow: hidden;
  }

  .statistic-item .icon-content {
    width: 50px;
    min-width: 50px;
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #3b4e82;
    border-radius: 50%;
  }

  .icon-content .iconfont {
    font-size: 24px;
    color: #fff;
  }

  .text-content {
    display: flex;
    margin-left: 10px;
    overflow: hidden;
    flex-direction: column;
    align-items: flex-start;
  }

  .statistic-item .text {
    font-size: 26px;
    color: #fff;
  }

  .statistic-item .sub-title {
    font-size: 12px;
    text-align: left;
    color: rgb(154, 168, 212);
  }

  .chart {
    height: 300px;
  }

  .echart {
    width: 500px;
    height: 500px;
  }



  .table-container {
    width: 400px;
    padding: 20px;
  }

  .table {
    width: 100%;
    background-color: transparent;
    color: #666;
    border-width: 1px;
    border-style: solid;
    border-color: #e6e6e6;
  }

  .table.darkblue {
    border-color: rgba(130, 157, 248, 0.705);
  }

  .table.darkblue td {
    border: none;
  }

  .table.darkblue tr .th {
    color: rgb(154, 168, 212);
  }

  .table.darkblue tr td {
    color: rgb(229, 233, 247);

  }

  .table tr {
    transition: all .3s;
    -webkit-transition: all .3s;
  }

  .table td.border_bottom_none {
    border-bottom: none;
  }

  .table td.td_2 {
    padding: 0;
  }

  .table td,
  .table th {
    position: relative;
    padding: 9px 15px;
    min-height: 20px;
    line-height: 20px;
    font-size: 14px;
    border-width: 1px;
    border-style: solid;
    border-color: #e6e6e6;
    padding: 5px 10px;
    border-top: none;
    border-left: none
  }

  .table td:last-child {
    border-right: none;
  }

  .table tr:last-child {
    border-bottom: none;
  }

  .table td.padding_none {
    padding: 0;
  }

  .width_half {
    width: 50%;
  }

  .td_2_table tr:last-child td:last-child {
    border-bottom: none;
  }

  .td_right {
    text-align: right;
  }

  .chart-container {
    height: 200px;
  }


  .box-content .caption {
    text-align: left;
    color: #fff;
  }

  small.number {
    color: #0b8603;
    font-weight: bold;
  }

  .warning {
    color: red;
  }
</style>
