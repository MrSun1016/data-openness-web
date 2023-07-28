<template>
  <div id="openEchearts">
    <!-- 头部 -->
    <img src="~@assets/images/背景.jpg" alt="" />
    <div class="conten">
      <div class="header">
        <img src="~@assets/images/标题.png" alt="" />
      </div>
      <div class="Main">
        <div class="leftbiao">
          <div class="leftbiao-top">
            <div class="img"></div>
            <div class="text">数据资源总览</div>
            <div class="natural">
              <div class="natural-box">
                <div class="title">数据项总量</div>
                <div class="naturalNum">30,761</div>
              </div>
              <div class="natural-box">
                <div class="title">开发目录总量</div>
                <div class="naturalNum">3,331</div>
              </div>
              <div class="natural-box">
                <div class="title">开发数据总量</div>
                <div class="naturalNum">15.7亿</div>
              </div>
              <div class="natural-box">
                <div class="title">接口总量</div>
                <div class="naturalNum">3,296</div>
              </div>
            </div>
          </div>
          <div class="leftbiao-center">
            <!-- <img src="~@assets/images/标题栏.png" alt="" /> -->
            <div class="img"></div>
            <div class="text">访问量(次)</div>
            <div class="visits">
              <div id="visitsecharts1"></div>
            </div>
          </div>
          <div class="leftbiao-bottom">
            <div class="img"></div>
            <div class="text">数据更新趋势</div>
            <div class="updateTrends">
              <el-tabs v-model="activeName" class="update-tabs">
                <el-tab-pane label="按月" name="first">
                  <div id="TrendsEcharts"></div>
                </el-tab-pane>
                <el-tab-pane label="按周" name="second">
                  <div id="weeklyEcharts"></div>
                </el-tab-pane>
                <el-tab-pane label="按日" name="third">
                  <div id="DailyEcharts"></div>
                </el-tab-pane>
              </el-tabs>
            </div>
          </div>
        </div>
        <div class="centerbiao">
          <div class="centerbiao-top">
            <!-- <div class="centerheader"> -->
            <div class="center-box">
              <div class="title">平台访问量(次)</div>
              <div class="naturalNum1">76975681</div>
            </div>
            <div class="center-box">
              <div class="title">接口调用量</div>
              <div class="naturalNum">15.7亿</div>
            </div>
            <div class="center-box">
              <div class="title">可下载文件数</div>
              <div class="naturalNum">19012</div>
            </div>
            <div class="center-box">
              <div class="title">文件下载量</div>
              <div class="naturalNum">696892</div>
            </div>
            <div class="center-box">
              <div class="title">市级部门/区</div>
              <div class="naturalNum">49</div>
            </div>
            <div class="center-box">
              <div class="title">应用成果总量</div>
              <div class="naturalNum">20</div>
            </div>
            <!-- </div> -->
          </div>
          <div class="centerbiao-bottom">
            <div class="img"></div>
            <div class="text">平台访问历史</div>
            <div class="platform">
              <div id="accessHistory"></div>
            </div>
          </div>
        </div>
        <div class="rightbiao">
          <div class="rightbiao-top">
            <div class="registryAnalysis">
              <div class="img"></div>
              <div class="text">注册类型分析</div>
              <div class="registry">
                <div id="Analysis"></div>
                <div id="typesOf"></div>
              </div>
            </div>
            <div class="heatWord">
              <div class="img"></div>
              <div class="text">关注热度词云</div>
              <div class="follow">
                <div id="WithInterest"></div>
              </div>
            </div>
          </div>
          <div class="rightbiao-center">
            <div class="img"></div>
            <div class="text">数据评分统计</div>
            <div class="dataRating">
              <div id="scoringStatistics"></div>
            </div>
          </div>
          <div class="rightbiao-bottom">
            <div class="img"></div>
            <div class="text">领域开发排行统计</div>
            <div class="development">
              <div id="DomainDevelopment"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'openEchearts',
  data() {
    return {
      palceDatas: ['北京', '广东', '浙江', '内蒙古', '四川', '河北', '上海', '江苏', '河南', '香港'],
      palceNum: [123, 456, 41, 242, 210, 395, 292, 143, 96, 638],
      activeName: 'first'
    }
  },
  mounted() {
    this.visitsEcharts()
    this.updateEchartsmonthly()
    this.updateEchartsweekly()
    this.updateEchartsDaily()
    this.wordCloudEcharts()
    this.analysisEcharts()
    this.typesOfEcharts()
    this.statisticsEcharts()
    this.domainDevelopmentEcharts()
    this.accessHistoryEcharts()
  },
  methods: {
    // 横向柱状图 访问量
    visitsEcharts() {
      var myChart = this.$echarts.init(document.getElementById('visitsecharts1'))
      var option = {
        grid: {
          top: '4%',
          left: '3%',
          right: '15%',
          bottom: '0',
          containLabel: true
        },
        xAxis: [
          //x轴数据设置
          {
            type: 'value',
            show: false
          }
        ],
        yAxis: [
          {
            type: 'category',
            offset: 0,
            axisLine: {
              show: false,
              //这是x轴文字颜色
              lineStyle: {
                color: '#fff'
              }
            },
            data: this.palceDatas,
            axisPointer: {
              type: 'shadow'
            },
            axisTick: {
              show: false
            }
          }
        ],
        series: [
          {
            data: this.palceNum,
            type: 'bar',
            label: {
              normal: {
                show: true
              }
            },
            itemStyle: {
              normal: {
                color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                  { offset: 0, color: 'rgba(27, 126, 242, 0.3)' }, //柱状图从左向右颜色渐变
                  { offset: 1, color: '#004cd8' }
                ]),
                label: {
                  textStyle: {
                    fontSize: '9', //柱状上的显示的文字
                    color: '#fff'
                  }
                }
              }
            }
          }
        ]
      }
      myChart.setOption(option)
    },
    // 数据更新趋势
    // 按月
    updateEchartsmonthly() {
      var myChart = this.$echarts.init(document.getElementById('TrendsEcharts'))
      var option = {
        grid: {
          top: '15%',
          left: '3%',
          right: '15%',
          bottom: '0',
          containLabel: true
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
          textStyle: {
            fontSize: 9
          },
          axisLabel: {
            //x轴文字的配置
            show: true,
            textStyle: {
              color: '#C9CBD3'
            }
          }
        },
        yAxis: {
          type: 'value',
          axisLabel: {
            fontSize: 10,
            //y轴文字的配置
            show: true,
            // fontSize: '13px',
            textStyle: {
              color: '#C9CBD3'
            }
          },
          splitLine: {
            show: true,
            lineStyle: {
              color: 'rgba(255,255,255, 0.3)',
              width: 1
              // type: 'dashed'
            }
          }
        },
        series: [
          {
            color: '#FF571A', //折线颜色
            symbolSize: 3, //折线点大小
            itemStyle: {
              normal: {
                label: {
                  show: true,
                  position: 'top',
                  textStyle: {
                    color: '#C9CBD3'
                  }
                }
              }
            },
            lineStyle: {
              // 设置线条的style等
              normal: {
                color: '#FF571A', // 折线线条颜色:红色
                width: 1
              }
            },
            //重点
            data: [820, 932, 901, 934, 1290, 1330, 1320],
            type: 'line',
            areaStyle: {
              normal: {
                color: {
                  x: 0,
                  y: 0,
                  x2: 0,
                  y2: 1,
                  colorStops: [
                    {
                      offset: 0.1,
                      color: '#FFC63A' // 0% 处的颜色
                    },
                    {
                      offset: 0.5,
                      color: 'rgba(255,198,58, 0.4)' // 0% 处的颜色
                    },
                    // {
                    //   offset: 0.7,
                    //   color: 'rgba(255,198,58, 0.2)' // 0% 处的颜色
                    // },
                    {
                      offset: 0.9,
                      color: 'rgba(216,216,216, 0.2)' // 100% 处的颜色
                    }
                  ],
                  globalCoord: false // 缺省为 false
                }
              }
            }
          }
        ]
      }
      myChart.setOption(option)
    },
    // 按周
    updateEchartsweekly() {
      var myChart = this.$echarts.init(document.getElementById('weeklyEcharts'))
      var option = {
        grid: {
          top: '15%',
          left: '3%',
          right: '15%',
          bottom: '0',
          containLabel: true
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
          textStyle: {
            fontSize: 9
          },
          axisLabel: {
            //x轴文字的配置
            show: true,
            textStyle: {
              color: '#C9CBD3'
            }
          }
        },
        yAxis: {
          type: 'value',
          axisLabel: {
            fontSize: 10,
            //y轴文字的配置
            show: true,
            // fontSize: '13px',
            textStyle: {
              color: '#C9CBD3'
            }
          },
          splitLine: {
            show: true,
            lineStyle: {
              color: 'rgba(255,255,255, 0.3)',
              width: 1
              // type: 'dashed'
            }
          }
        },
        series: [
          {
            color: '#FF571A', //折线颜色
            symbolSize: 3, //折线点大小
            itemStyle: {
              normal: {
                label: {
                  show: true,
                  position: 'top',
                  textStyle: {
                    color: '#C9CBD3'
                  }
                }
              }
            },
            lineStyle: {
              // 设置线条的style等
              normal: {
                color: '#FF571A', // 折线线条颜色:红色
                width: 1
              }
            },
            //重点
            data: [820, 932, 901, 934, 1290, 1330, 1320],
            type: 'line',
            areaStyle: {
              normal: {
                color: {
                  x: 0,
                  y: 0,
                  x2: 0,
                  y2: 1,
                  colorStops: [
                    {
                      offset: 0.1,
                      color: '#FFC63A' // 0% 处的颜色
                    },
                    {
                      offset: 0.5,
                      color: 'rgba(255,198,58, 0.4)' // 0% 处的颜色
                    },
                    // {
                    //   offset: 0.7,
                    //   color: 'rgba(255,198,58, 0.2)' // 0% 处的颜色
                    // },
                    {
                      offset: 0.9,
                      color: 'rgba(216,216,216, 0.2)' // 100% 处的颜色
                    }
                  ],
                  globalCoord: false // 缺省为 false
                }
              }
            }
          }
        ]
      }
      myChart.setOption(option)
    },
    // 按日
    updateEchartsDaily() {
      var myChart = this.$echarts.init(document.getElementById('DailyEcharts'))
      var option = {
        grid: {
          top: '15%',
          left: '3%',
          right: '15%',
          bottom: '0',
          containLabel: true
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
          textStyle: {
            fontSize: 9
          },
          axisLabel: {
            //x轴文字的配置
            show: true,
            textStyle: {
              color: '#C9CBD3'
            }
          }
        },
        yAxis: {
          type: 'value',
          axisLabel: {
            fontSize: 10,
            //y轴文字的配置
            show: true,
            // fontSize: '13px',
            textStyle: {
              color: '#C9CBD3'
            }
          },
          splitLine: {
            show: true,
            lineStyle: {
              color: 'rgba(255,255,255, 0.3)',
              width: 1
              // type: 'dashed'
            }
          }
        },
        series: [
          {
            color: '#FF571A', //折线颜色
            symbolSize: 3, //折线点大小
            itemStyle: {
              normal: {
                label: {
                  show: true,
                  position: 'top',
                  textStyle: {
                    color: '#C9CBD3'
                  }
                }
              }
            },
            lineStyle: {
              // 设置线条的style等
              normal: {
                color: '#FF571A', // 折线线条颜色:红色
                width: 1
              }
            },
            //重点
            data: [820, 932, 901, 934, 1290, 1330, 1320],
            type: 'line',
            areaStyle: {
              normal: {
                color: {
                  x: 0,
                  y: 0,
                  x2: 0,
                  y2: 1,
                  colorStops: [
                    {
                      offset: 0.1,
                      color: '#FFC63A' // 0% 处的颜色
                    },
                    {
                      offset: 0.5,
                      color: 'rgba(255,198,58, 0.4)' // 0% 处的颜色
                    },
                    // {
                    //   offset: 0.7,
                    //   color: 'rgba(255,198,58, 0.2)' // 0% 处的颜色
                    // },
                    {
                      offset: 0.9,
                      color: 'rgba(216,216,216, 0.2)' // 100% 处的颜色
                    }
                  ],
                  globalCoord: false // 缺省为 false
                }
              }
            }
          }
        ]
      }
      myChart.setOption(option)
    },
    // 关注热度词云
    wordCloudEcharts() {
      var myChart = this.$echarts.init(document.getElementById('WithInterest'))
      var keywords = [
        { name: '男神', value: 2.64 },
        { name: '好身材', value: 4.03 },
        { name: '正直', value: 24.95 },
        { name: '酷', value: 4.04 },
        { name: '时尚', value: 5.27 },
        { name: '阳光活力', value: 5.8 },
        { name: '初恋', value: 3.09 },
        { name: '英气', value: 14.71 },
        { name: '霸气', value: 6.33 },
        { name: '腼腆', value: 2.55 },
        { name: '蠢萌', value: 3.88 },
        { name: '青春', value: 8.04 },
        { name: '网红', value: 5.87 },
        { name: '萌', value: 6.97 },
        { name: '认真', value: 2.53 },
        { name: '古典', value: 2.49 },
        { name: '温柔', value: 3.91 },
        { name: '有个性', value: 3.25 },
        { name: '可爱', value: 9.93 },
        { name: '幽默诙谐', value: 3.65 }
      ]
      const colorList = ['#f89b6299', '#afcf7cde', '#fece5b']
      var option = {
        series: [
          {
            type: 'wordCloud',
            //maskImage: maskImage,
            sizeRange: [10, 25],
            rotationRange: [-30, 40],
            rotationStep: 45,
            gridSize: 8,
            shape: 'pentagon',
            width: '100%',
            height: '100%',
            textStyle: {
              normal: {
                color: function() {
                  let index = Math.floor(Math.random() * colorList.length)
                  return colorList[index]
                },
                fontFamily: 'sans-serif',
                fontWeight: 'normal'
              },
              emphasis: {
                shadowBlur: 10,
                shadowColor: '#333'
              }
            },
            data: keywords
          }
        ]
      }
      // 使用刚指定的配置项和数据显示图表。
      myChart.setOption(option)
    },
    // 注册类型分析
    analysisEcharts() {
      var myChart = this.$echarts.init(document.getElementById('Analysis'))
      var option = {
        tooltip: {
          trigger: 'item'
        },
        series: [
          {
            name: 'Access From',
            type: 'pie',
            radius: ['40%', '60%'],
            avoidLabelOverlap: false,
            label: {
              color: '#fff',
              fontSize: 9
            },
            labelLine: {
              //指示线样式设置
              normal: {
                length: 1 //设置指示线的长度
              }
            },

            data: [
              { value: 1048, name: 'Search Engine' },
              { value: 735, name: 'Direct' },
              { value: 580, name: 'Email' },
              { value: 484, name: 'Union Ads' },
              { value: 300, name: 'Video Ads' }
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ]
      }
      myChart.setOption(option)
    },
    // 注册类型分析 小圆
    typesOfEcharts() {
      var myChart = this.$echarts.init(document.getElementById('typesOf'))
      var option = {
        series: [
          {
            name: 'Access From',
            type: 'pie',
            radius: '50%',
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: 'center'
            },
            labelLine: {
              show: false
            },
            data: [
              { value: 484, name: '企业' },
              { value: 300, name: '个人' }
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ]
      }
      myChart.setOption(option)
    },
    // 数据评分统计
    statisticsEcharts() {
      var myChart = this.$echarts.init(document.getElementById('scoringStatistics'))
      var option = {
        tooltip: {
          trigger: 'item'
        },
        series: [
          {
            name: 'Access From',
            type: 'pie',
            radius: '70%',
            avoidLabelOverlap: false,
            label: {
              color: '#fff',
              fontSize: 9
            },
            labelLine: {
              //指示线样式设置
              normal: {
                length: 1 //设置指示线的长度
              }
            },

            data: [
              { value: 1048, name: 'Search Engine' },
              { value: 735, name: 'Direct' },
              { value: 580, name: 'Email' },
              { value: 484, name: 'Union Ads' },
              { value: 300, name: 'Video Ads' }
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ]
      }
      myChart.setOption(option)
    },
    // 领域开发排行统计
    domainDevelopmentEcharts() {
      var myChart = this.$echarts.init(document.getElementById('DomainDevelopment'))
      var option = {
        tooltip: {
          show: false
        },
        legend: {
          orient: 'vertical',
          left: 'left',
          padding: [30, 0, 0, 0],
          textStyle: {
            color: '#FFF'
          },
          icon: 'pin'
        },
        // grid: {
        //   right: '30%'
        // },
        series: [
          {
            name: '数据比例',
            type: 'pie',
            radius: ['55%', '80%'],
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: 'center',
              formatter: '{b}: {c}'
            },
            emphasis: {
              label: {
                show: true,
                fontSize: 20,
                fontWeight: 'bold'
              }
            },
            labelLine: {
              show: false
            },
            data: [
              { value: 1048, name: 'Search Engine' },
              { value: 735, name: 'Direct' },
              { value: 580, name: 'Email' },
              { value: 484, name: 'Union Ads' },
              { value: 300, name: 'Video Ads' }
            ],
            // data: this.pieshapedcomparison,
            center: ['60%', '50%']
          }
        ]
      }
      myChart.setOption(option)
    },
    // 平台访问历史
    accessHistoryEcharts() {
      var myChart = this.$echarts.init(document.getElementById('accessHistory'))
      // 指定图表的配置项和数据
      var option = {
        // title: {
        //   text: "Stacked Area Chart",
        // },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'line',
            label: {
              backgroundColor: '#6a7985'
            }
          },
          textStyle: {
            color: '#FFF'
          },
          borderColor: '#1B80F7',
          backgroundColor: 'rgba(0,0,0, 0.5)'
        },
        legend: {
          right: 'auto',
          x: 'center',
          y: 'top',
          textStyle: {
            color: '#FFF'
          },
          icon: 'roundRect',
          itemHeight: 2, // 粗细
          data: ['访问量', '下载量', '调用量']
        },
        grid: {
          top: '15%',
          left: '3%',
          right: '15%',
          bottom: '3%',
          containLabel: true
        },

        xAxis: [
          {
            type: 'category',
            boundaryGap: false,
            // fontSize: '13px',
            axisLabel: {
              //x轴文字的配置
              show: true,
              textStyle: {
                color: '#C9CBD3'
              }
            },

            data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
          }
        ],
        yAxis: [
          {
            // name: "单位:条",
            nameTextStyle: {
              color: '#fff',
              nameLocation: 'start',
              fontSize: 13
            },
            axisLabel: {
              //y轴文字的配置
              show: true,
              // fontSize: '13px',
              textStyle: {
                color: '#C9CBD3'
              }
            },
            splitLine: {
              show: true,
              lineStyle: {
                color: 'rgba(255,255,255, 0.3)',
                width: 1
                // type: 'dashed'
              }
            },
            type: 'value'
          }
        ],
        series: [
          {
            name: '访问量',
            smooth: true,
            type: 'line',
            // stack: "总量1",
            itemStyle: {
              color: '#2AF9FF' //小圆点和线的颜色
            },
            areaStyle: {
              normal: {
                color: {
                  x: 0,
                  y: 0,
                  x2: 0,
                  y2: 1,
                  colorStops: [
                    {
                      offset: 0.1,
                      color: '#2AF9FF' // 0% 处的颜色
                    },
                    {
                      offset: 0.5,
                      color: 'rgba(42,249,255, 1)' // 0% 处的颜色
                    },
                    {
                      offset: 0.7,
                      color: 'rgba(42,249,255, 0.7)' // 0% 处的颜色
                    },
                    {
                      offset: 0.9,
                      color: 'rgba(42,249,255, 0.2)' // 100% 处的颜色
                    }
                  ],
                  globalCoord: false // 缺省为 false
                }
              }
            },
            emphasis: {
              focus: 'series'
            },
            data: [820, 932, 901, 934, 1290, 1330, 1320]
          },
          {
            name: '下载量',
            smooth: true,
            type: 'line',
            // stack: "总量2",
            itemStyle: {
              color: '#1B80F7' //小圆点和线的颜色
            },
            areaStyle: {
              normal: {
                color: {
                  x: 0,
                  y: 0,
                  x2: 0,
                  y2: 1,
                  colorStops: [
                    {
                      offset: 0.1,
                      color: '#1B80F7' // 0% 处的颜色
                    },
                    {
                      offset: 0.5,
                      color: 'rgba(27,129,249, 0.9)' // 0% 处的颜色
                    },
                    {
                      offset: 0.7,
                      color: 'rgba(27,129,249, 0.7)' // 0% 处的颜色
                    },
                    {
                      offset: 0.9,
                      color: 'rgba(27,129,2495, 0.2)' // 100% 处的颜色
                    }
                  ],
                  globalCoord: false // 缺省为 false
                }
              }
            },
            emphasis: {
              focus: 'series'
            },
            data: [820, 453, 354, 234, 725, 3453, 343]
          },
          {
            name: '调用量',
            smooth: true,
            type: 'line',
            // stack: "总量2",
            itemStyle: {
              color: '#CF070D' //小圆点和线的颜色
            },
            areaStyle: {
              normal: {
                color: {
                  x: 0,
                  y: 0,
                  x2: 0,
                  y2: 1,
                  colorStops: [
                    {
                      offset: 0.1,
                      color: '#CF070D' // 0% 处的颜色
                    },
                    {
                      offset: 0.5,
                      color: 'rgba(207,7,13, 0.9)' // 0% 处的颜色
                    },
                    {
                      offset: 0.7,
                      color: 'rgba(207,7,13, 0.7)' // 0% 处的颜色
                    },
                    {
                      offset: 0.9,
                      color: 'rgba(207,7,13, 0.2)' // 100% 处的颜色
                    }
                  ],
                  globalCoord: false // 缺省为 false
                }
              }
            },
            emphasis: {
              focus: 'series'
            },
            data: [390, 453, 254, 234, 935, 353, 553]
          }
        ]
      }
      myChart.setOption(option)
    }
  }
}
</script>

<style lang="less" scoped>
#openEchearts {
  width: 100%;
  height: 100%;
  background-color: black;
  position: relative;
  // background-image: url(~@/assets/images/背景.jpg);
  // background-size: 100% 100%;
  text-align: center;
  img {
    width: 78%;
    height: 100%;
  }
  .conten {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    .header {
      width: 100%;
      height: 10%;
      // background-color: rgba(255, 218, 218, 0.5);
      img {
        width: 100%;
        height: 55px;
      }
    }
    .Main {
      width: 100%;
      height: 90%;
      color: #fff;
      padding: 0 15px;
      font-size: 13px;
      display: flex;
      justify-content: space-between;
      // background-color: rgba(146, 146, 146, 0.5);
      .leftbiao {
        width: 24%;
        height: 100%;
        // background-color: rgba(184, 232, 238, 0.5);
        .leftbiao-top {
          width: 100%;
          height: 18%;
          position: relative;
          .img {
            width: 100%;
            height: 25px;
            margin-bottom: 5px;
            background-image: url(~@/assets/images/标题栏.png);
            background-size: 100% 100%;
          }
          .text {
            font-size: 15px;
            position: absolute;
            top: 3px;
            left: 35px;
          }
          .natural {
            width: 100%;
            height: 70%;
            display: flex;
            flex-direction: row;
            justify-content: space-around;
            background: linear-gradient(to bottom, rgba(27, 126, 242, 0.3), rgba(255, 255, 255, 0));
            .natural-box {
              text-align: center;
              .title {
                margin-top: 10px;
              }
              .naturalNum {
                font-size: 21px;
                background-image: -webkit-linear-gradient(bottom, #004cd8, #cfdefc);
                -webkit-background-clip: text;
                -webkit-text-fill-color: transparent;
              }
            }
          }
        }
        .leftbiao-center {
          width: 100%;
          height: 41%;
          position: relative;
          .img {
            width: 100%;
            height: 25px;
            margin-bottom: 5px;
            background-image: url(~@/assets/images/标题栏.png);
            background-size: 100% 100%;
          }
          .text {
            font-size: 15px;
            position: absolute;
            top: 3px;
            left: 35px;
          }
          .visits {
            width: 100%;
            height: 83%;
            // background-color: rgba(21, 154, 172, 0.5);
            background: linear-gradient(to bottom, rgba(27, 126, 242, 0.3), rgba(255, 255, 255, 0));
            #visitsecharts1 {
              width: 100%;
              height: 100%;
            }
          }
        }
        .leftbiao-bottom {
          width: 100%;
          height: 41%;
          position: relative;
          .img {
            width: 100%;
            height: 25px;
            margin-bottom: 5px;
            background-image: url(~@/assets/images/标题栏.png);
            background-size: 100% 100%;
          }
          .text {
            font-size: 15px;
            position: absolute;
            top: 3px;
            left: 35px;
          }
          .updateTrends {
            width: 100%;
            height: 83%;
            padding: 5px;
            background: linear-gradient(to bottom, rgba(27, 126, 242, 0.3), rgba(255, 255, 255, 0));
          }
        }
      }
      .centerbiao {
        width: 50%;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        // background-color: rgba(201, 171, 230, 0.5);
        .centerbiao-top {
          width: 100%;
          height: 15%;
          display: flex;
          flex-direction: row;
          justify-content: space-around;
          // background-color: rgba(139, 75, 199, 0.5);
          .center-box {
            text-align: center;
            .title {
              margin-top: 10px;
            }
            .naturalNum {
              font-size: 24px;
              font-weight: 500;
              background-image: -webkit-linear-gradient(bottom, #004cd8, #cfdefc);
              -webkit-background-clip: text;
              -webkit-text-fill-color: transparent;
            }
            .naturalNum1 {
              font-size: 24px;
              font-weight: 500;
              background-image: -webkit-linear-gradient(bottom, #ffdb25, #cfdefc);
              -webkit-background-clip: text;
              -webkit-text-fill-color: transparent;
            }
          }
        }
        .centerbiao-bottom {
          width: 100%;
          height: 38%;
          // background-color: rgba(64, 32, 94, 0.5);
          position: relative;
          .img {
            width: 45%;
            height: 25px;
            margin-bottom: 5px;
            background-image: url(~@/assets/images/标题栏.png);
            background-size: 100% 100%;
          }
          .text {
            font-size: 15px;
            position: absolute;
            top: 3px;
            left: 33px;
          }
          .platform {
            width: 100%;
            height: 83%;
            padding: 5px;
            background: linear-gradient(to bottom, rgba(27, 126, 242, 0.3), rgba(255, 255, 255, 0));
          }
        }
      }
      .rightbiao {
        width: 24%;
        height: 100%;
        // background-color: rgba(165, 226, 141, 0.5);
        .rightbiao-top {
          width: 100%;
          height: 30%;
          display: flex;
          justify-content: space-between;
          // background-color: rgba(95, 160, 168, 0.5);
          .registryAnalysis {
            width: 49%;
            height: 100%;
            position: relative;
            .img {
              width: 100%;
              height: 25px;
              margin-bottom: 5px;
              background-image: url(~@/assets/images/标题栏1.png);
              background-size: 100% 100%;
            }
            .text {
              font-size: 15px;
              position: absolute;
              top: 3px;
              left: 35px;
            }
            .registry {
              width: 100%;
              height: 83%;
              padding: 5px;
              position: relative;
              background: linear-gradient(to bottom, rgba(27, 126, 242, 0.3), rgba(255, 255, 255, 0));
            }
          }
          .heatWord {
            width: 49%;
            height: 100%;
            position: relative;
            .img {
              width: 100%;
              height: 25px;
              margin-bottom: 5px;
              background-image: url(~@/assets/images/标题栏1.png);
              background-size: 100% 100%;
            }
            .text {
              font-size: 15px;
              position: absolute;
              top: 3px;
              left: 35px;
            }
            .follow {
              width: 100%;
              height: 83%;
              padding: 5px;
              background: linear-gradient(to bottom, rgba(27, 126, 242, 0.3), rgba(255, 255, 255, 0));
            }
          }
        }
        .rightbiao-center {
          width: 100%;
          height: 30%;
          position: relative;
          .img {
            width: 100%;
            height: 25px;
            margin-bottom: 5px;
            background-image: url(~@/assets/images/标题栏.png);
            background-size: 100% 100%;
          }
          .text {
            font-size: 15px;
            position: absolute;
            top: 3px;
            left: 35px;
          }

          .dataRating {
            width: 100%;
            height: 83%;
            padding: 5px;
            background: linear-gradient(to bottom, rgba(27, 126, 242, 0.3), rgba(255, 255, 255, 0));
          }
        }
        .rightbiao-bottom {
          width: 100%;
          height: 40%;
          // background-color: rgba(18, 76, 83, 0.5);
          position: relative;
          .img {
            width: 100%;
            height: 25px;
            margin-bottom: 5px;
            background-image: url(~@/assets/images/标题栏.png);
            background-size: 100% 100%;
          }
          .text {
            font-size: 15px;
            position: absolute;
            top: 3px;
            left: 35px;
          }
          .development {
            width: 100%;
            height: 83%;
            padding: 5px;
            background: linear-gradient(to bottom, rgba(27, 126, 242, 0.3), rgba(255, 255, 255, 0));
          }
        }
      }
    }
  }
}
#TrendsEcharts {
  width: 350px;
  height: 160px;
  // background-color: rgba(18, 76, 83, 0.5);
}
#weeklyEcharts {
  width: 350px;
  height: 160px;
  // background-color: rgba(18, 76, 83, 0.5);
}
#DailyEcharts {
  width: 350px;
  height: 160px;
  // background-color: rgba(18, 76, 83, 0.5);
}
#Analysis {
  width: 100%;
  height: 100%;
}
#typesOf {
  width: 50%;
  height: 50%;
  position: absolute;
  top: 25%;
  left: 25%;
}
#WithInterest {
  width: 100%;
  height: 100%;
  // background-color: rgba(21, 154, 172, 0.5);
}
#scoringStatistics {
  width: 100%;
  height: 100%;
  // background-color: rgba(21, 154, 172, 0.5);
}
#DomainDevelopment {
  width: 100%;
  height: 100%;
  // background-color: rgba(21, 154, 172, 0.5);
}
#accessHistory {
  width: 100%;
  height: 100%;
}
.update-tabs {
  height: 100%;
}
.el-tabs__content {
  height: 100% !important;
  /* overflow-y: auto; */
}

/deep/.el-tabs__nav-wrap::after {
  background: #1767ff !important;
}
/deep/.el-tabs__item.is-active {
  background: linear-gradient(to bottom, #b6bfd1, #1351c5);
}
/deep/ .el-tabs__item {
  padding: 0px;
  width: 40px;
  height: 25px;
  line-height: 25px;
  color: #fff;
  font-size: 13px;
  text-align: center;
}
/deep/ .el-tabs__active-bar {
  width: 40px !important;
}
/deep/ .el-tabs__header {
  margin: 0;
}
</style>
