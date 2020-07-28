<template>
  <div class="chart-wrapper">
    <echarts :options="chartOptions"/>
  </div>
</template>

<script>
  // https://echarts.apache.org/examples/en/index.html#chart-type-radar
  // https://echarts.apache.org/en/option.html#radar.indicator.name
  import Echarts from 'vue-echarts'
  import 'echarts/lib/chart/radar'     // /chart/radar СООТВЕТСТВУЕТ ТРЕБУЕМОМУ ТИПУ ГРАФИКА
  import 'echarts/lib/component/tooltip'
  
  export default {
    components: {
      Echarts
    },
    props: {
      indicatorNames: {
        type: Array,
        required: true
      },
      knowledgeChartData: {
        type: Array,
        required: true
      },
      interestsChartData: {
        type: Array,
        required: true
      },
    },
    computed: {
      chartParameters() {
        if(this.windowInnerWidth > 1024)
          return {
            chartRadius: ['0%', '55%'],
            axisLineSymbol: ['none', 'arrow'],
            nameGapValue: 40,
            axisLabelShow: true,     //цифра у зон
            labelShow: true,         //цифра у точек
            chartFontSize: 16,
            paddingTop: 10,
            paddingDown: 10
          }
        
        if(this.windowInnerWidth <= 412)
          return {
            chartRadius: ['0%', '30%'],
            axisLineSymbol: ['none', 'none'],
            nameGapValue: 15,
            axisLabelShow: false,
            labelShow: false,
            chartFontSize: 12,
            paddingTop: 5,
            paddingDown: -10
          }
        
        if(this.windowInnerWidth <= 566)     //414
          return {
            chartRadius: ['0%', '43%'],
            axisLineSymbol: ['none', 'none'],
            nameGapValue: 15,
            axisLabelShow: false,
            labelShow: true,
            chartFontSize: 12,
            paddingTop: 15,
            paddingDown: 0
          }
        
        if(this.windowInnerWidth <= 600)
          return {
            chartRadius: ['0%', '43%'],
            axisLineSymbol: ['none', 'arrow'],
            nameGapValue: 30,
            axisLabelShow: true,
            labelShow: true,
            chartFontSize: 16,
            paddingTop: 15,
            paddingDown: 0
          }
        if(this.windowInnerWidth <= 666)
          return {
            chartRadius: ['0%', '43%'],
            axisLineSymbol: ['none', 'arrow'],
            nameGapValue: 40,
            axisLabelShow: true,
            labelShow: true,
            chartFontSize: 16,
            paddingTop: 15,
            paddingDown: 0
          }
        if(this.windowInnerWidth <= 766)
          return {
            chartRadius: ['0%', '50%'],
            axisLineSymbol: ['none', 'arrow'],
            nameGapValue: 40,
            axisLabelShow: true,
            labelShow: true,
            chartFontSize: 16,
            paddingTop: 15,
            paddingDown: 0
          }
        if(this.windowInnerWidth <= 894)
          return {
            chartRadius: ['0%', '50%'],
            axisLineSymbol: ['none', 'arrow'],
            nameGapValue: 40,
            axisLabelShow: true,
            labelShow: true,
            chartFontSize: 16,
            paddingTop: 15,
            paddingDown: 0
          }
        if(this.windowInnerWidth <= 1020)
          return {
            chartRadius: ['0%', '50%'],
            axisLineSymbol: ['none', 'arrow'],
            nameGapValue: 40,
            axisLabelShow: true,
            labelShow: true,
            chartFontSize: 16,
            paddingTop: 15,
            paddingDown: 0
          }
        
        return {
          chartRadius: ['0%', '60%'],
          axisLineSymbol: ['none', 'arrow'],
          nameGapValue: 40,
          axisLabelShow: true,
          labelShow: true,
          chartFontSize: 16,
          paddingTop: 15,
          paddingDown: 0
        }
      },
      chartOptions() {
        //добавление "\n" после каждого слова в имя индикатора.
        let indicatorNames_mobile = []
        if(this.windowInnerWidth < 1025) {
          for(let indicatorName of this.indicatorNames) {
            let newIndicatorName = ''
            let nameWords = indicatorName.split(' ')
            
            for(let word of nameWords) {
              let newIndicatorWord = ''
              if(word !== 'и') {
                newIndicatorWord = word + '\n'
              } else {
                newIndicatorWord = word + ' '
              }
              newIndicatorName = newIndicatorName + newIndicatorWord
            }
            indicatorNames_mobile.push(newIndicatorName)
          }
        }
        
        //формирование {} для indicator-параметра.
        let indicator = []
        if(this.windowInnerWidth < 1025) {
          for(let item of indicatorNames_mobile) {
            indicator.push({name: item, max: 100, color: '#fff'})  //цвет текста common.
          }
        } else {
          for(let item of this.indicatorNames) {
            indicator.push({name: item, max: 100, color: '#fff'})  //цвет текста common.
          }
        }
        
        return {
          // grid: [{     //no work
          //  width: '80%' ,
          //  height: 'auto' ,
          //  left: '10%' ,
          // }],
          radar: {
            radius: this.chartParameters.chartRadius,  //'50%' - et vw, ['0%', '70%'] для 700pxX700px, где '0%'- размер дырки в центре графика.
            shape: 'polygon',          //'polygon', 'circle'
            name: {
              //formatter: '= {value} = {a} = {b} = {c}',  //форматирование надписи
              textStyle: {
                width: '100%',    //Width of the text block.
                color: '#fff',    //перетирается параметром color в "indicator'e"
                fontWeight: 'bold',
                fontSize: this.chartParameters.chartFontSize,
                fontFamily: 'sans-serif',
                lineHeight: 16,
                backgroundColor: 'transparent',
                padding: [
                  this.chartParameters.paddingTop,   // top
                  15,                                // right
                  this.chartParameters.paddingDown,  // down
                  15                                 // left
                ],
                borderColor: 'rgba(235,236,237,0.2)',
                borderWidth: 1,
                borderRadius: 30,
                shadowColor: 'yellow',     //обводка-тень у border
                shadowBlur: 0,
                shadowOffsetX: 0,
                shadowOffsetY: 0,
                textBorderColor: 'transparent',   //обводка-тень у букв
                textBorderWidth: 3,
                textShadowColor: 'transparent',
                textShadowBlur: 0,
                textShadowOffsetX: 0,
                textShadowOffsetY: 0,
              }
            },
            nameGap: this.chartParameters.nameGapValue,   //растояние от имени линии до графика вдоль оси линии.
            indicator,
            center: ['50%', '57%'],  //расположение в блоке
            startAngle: 90,          //кручение Asc, против часовой стрелки
            splitNumber: 5,          //кол-во секторов
            splitArea: {             //дизайн секторов
              areaStyle: {
                color: ['#848098',
                  '#6B6784', '#524D6F',
                  '#3A345B', '#211A46'],
                // shadowColor: 'rgba(246,9,77,0.3)',
                // shadowBlur: 10    //тень у границы зонового кольца
              }
            },
            axisLine: {
              symbol: this.chartParameters.axisLineSymbol,   //стрелка, и только у дистального конца axis'a
              symbolSize: [11, 14],          //размер стрелки
              symbolOffset: [10, 25],        //вынос стрелки
              lineStyle: {
                color: 'rgba(255, 255, 255, 0.2)',
                type: 'dashed',
                width: 1,
              }
            },
            axisTick: {         //поперечные засечки на Axis
              show: false,
              length: 15
            },
            axisLabel: {       //цыфры-величина значений зон вдоль axis-линии
              show: this.chartParameters.axisLabelShow
            },
            splitLine: {
              lineStyle: {
                color: 'transparent'
              }
            },
            tooltip: {},
            triggerEvent: true       // no work
            // triggerEvent: {
            //   // Component type: xAxis, yAxis, radiusAxis, angleAxis
            //   // Each of which has an attribute for index, e.g., xAxisIndex for xAxis
            //   componentType: axisLine,
            //   // Value on axis before being formatted.
            //   // Click on value label to trigger event.
            //   value: '',
            //   // Name of axis.
            //   // Click on laben name to trigger event.
            //   name: ''
            // }
          },
          series: [{
            textStyle: {},   //common
            color: ['white', 'white'], //цвета ободков у ТОЧЕК, применяются по порядку заявления {} в series-массиве. У нас: Знание, Интерес.
            name: 'привет',
            type: 'radar',
            label: {
              // show: true    //подсказочка-цифра, значение точки
            },
            itemStyle: {
              color: 'red',
              // color: {
              //   image: imageDom, // HTMLImageElement, and HTMLCanvasElement are supported, while string path is not supported
              //   repeat: 'repeat' // whether to repeat texture, whose value can be repeat-x, repeat-y, or no-repeat
              // }
            },
            emphasis: {
              lineStyle: {
                width: 1,      //толщина линии при наведении
                color: 'blue'  //цвет при наведании
              },
              itemStyle: {},    //оно может быть и в data[].
              areaStyle: {},
              label: {}
            },
            data: [
              {   //ЗНАНИЯ
                value: this.knowledgeChartData,
                name: 'knowledge',
                symbol: 'circle',      //форма точек - 'circle', 'rect'.
                symbolSize: 7,         //размер точек
                lineStyle: {           //линия между точками
                  width: 0,
                  color: 'blue',
                  type: 'dashed'
                },
                areaStyle: {
                  color: 'rgba(109,76,204,0.6)',   //заливка зоны (знания).
                  //type: 'dashed'
                },
                label: {        //подсказка-значение точки. Тем же цветом, что и точка, по-умолчанию.
                  show: this.chartParameters.labelShow,
                  position: 'right',
                  color: '#fff'
                },
                itemStyle: {     //ЦВЕТ ТОЧЕК Индивидуально, их заливка. См. также color в корне series-объекта.
                  
                  // color: {   //вставляется не сюда, а рядом с data.
                  //     image: imageDom, // HTMLImageElement, and HTMLCanvasElement are supported, while string path is not supported
                  //     repeat: 'repeat' // whether to repeat texture, whose value can be repeat-x, repeat-y, or no-repeat
                  // },
                  
                  color: '#6D4CCC',   //заливка внутри точки
                  borderColor: '#fff',  //цвет ободка
                  borderType: 'solid',
                  borderWidth: 1,     //толщина ободка
                  shadowColor: 'rgba(245,7,74, 1)',  //ободок вокруг бордера точки ?
                  shadowBlur: 25,   //?
                  shadowOffsetX: 0,
                  shadowOffsetY: 0,
                },
                emphasis: {     //ПРИ НАВЕДЕНИИ
                  lineStyle: {
                    width: 1,      //толщина линии при наведении
                    color: 'blue'  //цвет при наведании
                  },
                  itemStyle: {},
                  areaStyle: {},
                  label: {}
                },
              },
              {   //ИНТЕРЕС
                value: this.interestsChartData,
                name: 'interests ',
                symbol: 'circle',      //форма точек - 'circle', 'rect'.
                symbolSize: 7,         //размер точек
                lineStyle: {           //линия между точками
                  width: 0,
                  color: 'blue',
                  type: 'dashed'
                },
                areaStyle: {
                  color: 'rgba(190,82,242,0.6)',   //заливка зоны (интерес).
                  //type: 'dashed'
                },
                label: {    //подсказка-значение точки. Тем же цветом, что и точка, по-умолчанию.
                  show: this.chartParameters.labelShow,
                  position: 'right',
                  color: '#fff'
                },
                itemStyle: {     //ЦВЕТ ТОЧЕК Индивидуально, их заливка. См. также color в корне series-объекта.
                  color: 'rgba(190,82,242,0.6)',   //внутри точки, её заливка
                  borderColor: '#fff',  //цвет ободка
                  borderType: 'solid',
                  borderWidth: 1,     //толщина ободка
                  shadowColor: 'rgba(245,7,74, 1)',  //ободок вокруг бордера точки ?
                  shadowBlur: 25   //?
                },
                emphasis: {     //ПРИ НАВЕДЕНИИ
                  lineStyle: {
                    width: 3,         //толщина линии при наведении
                    color: '#67039a'  //цвет линии между точками при наведании
                  },
                  itemStyle: {},
                  areaStyle: {},
                  label: {}
                },
              },
            ]
          }]
        }
      }
    },
    data: () => ({
      windowInnerWidth: null
    }),
    created() {
      this.windowInnerWidth = window.innerWidth
    }
  }
</script>

<style scoped lang="scss">
  .chart-wrapper {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    
    .echarts {
      width: 1024px; // >1024x    АНАЛОГ grid-параметра в chartOptions
      height: 730px; // >1024x
      
      @media (max-width: 768px) {
        width: 768px;
        height: 760px;
      }
      @media (max-width: 1024px) and (max-height: 768px) {
        width: 768px;
        height: 760px;
      }
      
      @media (max-width: 414px) {
        width: 414px;
        height: 410px;
      }
      @media (max-width: 896px) and (max-height: 414px) {
        width: 414px;
        height: 410px;
      }
      
      @media (max-width: 320px) {
        width: 320px;
        height: 300px;
      }
      @media (max-width: 568px) and (max-height: 320px) {
        width: 320px;
        height: 300px;
      }
    }
  }
</style>