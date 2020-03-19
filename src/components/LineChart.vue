<template>
    <div>
        <button ref="btn" @click="update_city('seoul')">seoul</button>
        <button ref="btn" @click="update_city('busan')">busan</button>
        <button ref="btn" @click="update_city('jeju')">jeju</button>
        <canvas ref="lineChart" width="400" height="400"></canvas>
    </div>    
</template>

<script>

export default {
    props:['dataset','labels'],
    data(){
        return{
            linechart:null
        }
    },
    methods:{
        update_city(city){
            this.$emit('update_city',city);
        },        
        uvcolor(value){
            var color;
            if(value < 3){
                color = 'green';
            }else if(value < 6){
                color = 'yellow';
            }else if(value < 8){
                color = 'orange';
            }else if(value < 11){
                color = 'red';
            }else{
                color = 'violet';
            }
            return color;
        },
        uvcolorarray(arr){            
            var uvcolor_arr = [];
            uvcolor_arr = arr.map(el => this.uvcolor(el));
            return uvcolor_arr;
        }
    },
    mounted(){        
        var ctx = this.$refs.lineChart;
        this.linechart = new this.$_Chart(ctx, {
            type: 'line',
            data: {
                labels: this.labels,
                datasets: [{
                            label: 'UV',
                            data: this.dataset,
                            borderColor: this.uvcolorarray(this.dataset),
                            borderWidth: 1
                        }]
            },
            options: {
                scales: {
                    yAxes: [{
                        ticks: {
                            beginAtZero: false
                        }
                    }]
                }
            }
        });
    },
    watch:{
        dataset:function(val){            
            this.linechart.data.datasets[0].data = val;
            this.linechart.data.datasets[0].borderColor = this.uvcolorarray(val);            
            this.linechart.update();
        },
        labels:function(val){
            this.linechart.data.labels = val;
            this.linechart.update();
        }
    }
}
</script>

<style>

</style>
