

<script>
import {Doughnut, mixins } from 'vue-chartjs'
const { reactiveProp } = mixins
export default {
  extends: Doughnut,
  mixins: [reactiveProp],
  data(){
    return{
        options: {
            responsive: true,
            maintainAspectRatio: true,
            legend: false,
            tooltips:{
              enabled:false
            },
            cutoutPercentage:'80'
            
            
          }
    }
  },
 
  mounted () {
    this.renderChart(this.chartData, this.options)
  }
}
</script>

