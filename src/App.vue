<script>
	import axios from "axios";
  export default{
		data() {
			// 页面数据变量
			return {
      }
		},
		methods:{
      isError(){
        let value = 0;
        for (let i = 1; i <= 5; i++) {
          value = Number(document.getElementById('mark_'+i).value)
          if(value>100||value<0){
            window.alert('The parameter is incorrect')
            return 0;
          }
          if(!value){
            window.alert('The Parameter is null')
            return 0 ;
          }
        }
      },
      save(type){
        let value = []
        let keyname= []
        for (let i = 1; i <= 5; i++) {
          value[i] = String(document.getElementById('module_' + i).value)
          keyname[i] = Number(document.getElementById('mark_' + i).value)
        }

        let myurl = 'https://tcb-3in63fdf4rxnza2-5ctz5777b530.service.tcloudbase.com/http/router/client/pub/hw'
        let xhttp = new XMLHttpRequest();
        xhttp.onreadystatechange = function() {
          if (this.readyState == 4 && this.status == 200) {
            var j = JSON.parse(this.response);
            let max_module = j.max_module;
            let min_module = j.min_module;

            document.getElementById('output-text').value = 'Highest scoring module = ' + max_module
                + '\nLowest scoring module = ' + min_module;
          }
        };
        xhttp.open("POST",myurl+"?value="+value+"&key="+keyname+"&type="+type);
        xhttp.send();

      },
      Api(type){
        if(this.isError() === 0){
          return;
        }
        if(type === 'getPassNumber')this.getPassNumber()
        if(type === 'getAverage') this.getAverage()
        if(type === 'getClassification') this.getClassification()
        if(type === 'getTotalMarks') this.getTotalMarks()
        if(type === 'getTotalMarks') this.getTotalMarks()
        if(type === 'getSortedModules') this.getSortedModules()

        this.save(type)
      },
      getPassNumber(){
        let value = 0
        let markGrade = []
        let count = 0
        for (let i = 1; i <= 5; i++) {
          value = Number(document.getElementById('mark_' + i).value)
          if (value > 100 || value < 0) {
            markGrade = 'mark ' + i + " numerical error"
          } else {
            if(value >=60)
              count++
          }
        }
        document.getElementById('output-text').value = 'Pass Count: '+count;

      },
      getAverage(){
        let ave = 0
        let value = []
        let number = 0
        for (let i = 1; i <= 5; i++) {
          value[i] = Number(document.getElementById('mark_' + i).value)
        }

        value.map(function (value,index) {
          ave+=value
        })
        number =  ave/5
        document.getElementById('output-text').value = 'Average: '+number;
      },
      //getClassification Button
      getClassification(){
        let value = 0
        let markGrade = []
        for (let i = 1; i <= 5; i++) {
          value = Number(document.getElementById('mark_'+i).value)
          if(value>100||value<0){
            markGrade =  'mark '+i+" numerical error"
          }
          else
          {
            let grade = ['Excellent','above average','average','usually the minimum passing grade','Fail']
            if(!value){
              markGrade[i] = 'value does not exist'
            }else{
              if(value>=90){
                markGrade[i] = grade[0]
              }else if(value>=80){
                markGrade[i] = grade[1]
              }else if(value>=70)
                markGrade[i] = grade[2]
              else if(value>=60)
                markGrade[i] = grade[3]
              else if(value<60)
                markGrade[i] = grade[4]
            }
          }

          console.log(markGrade)
          let arr = []
          markGrade.map(function(value,index) {
              arr[index] = 'makr '+index+' '+value+'\n'
          })
          document.getElementById('output-text').value = arr;
        }
      },

      //getTotalMarks Button
      getTotalMarks(){
        console.log(this.save(1))
        let count = 0
        for (let i = 1; i <= 5; i++) {
          count+= Number(document.getElementById('mark_'+i).value)
          console.log(count)
          document.getElementById('output-text').value = 'Total marks = ' + count;
        }
      },
      getMaxMin()
      {
        let maxminURL = "http://maxmin.esha.qpc.hal.davecutting.uk/";
        let module_1 = document.getElementById('module_1').value
        let module_2 = document.getElementById('module_2').value
        let module_3 = document.getElementById('module_3').value
        let module_4 = document.getElementById('module_4').value
        let module_5 = document.getElementById('module_5').value

        let mark_1 = document.getElementById('mark_1').value
        let mark_2 = document.getElementById('mark_2').value
        let mark_3 = document.getElementById('mark_3').value
        let mark_4 = document.getElementById('mark_4').value
        let mark_5 = document.getElementById('mark_5').value

        let xhttp = new XMLHttpRequest();
        xhttp.onreadystatechange = function() {
          if (this.readyState == 4 && this.status == 200) {
            var j = JSON.parse(this.response);
            let max_module = j.max_module;
            let min_module = j.min_module;

            document.getElementById('output-text').value = 'Highest scoring module = ' + max_module
                + '\nLowest scoring module = ' + min_module;
          }
        };
        xhttp.open("GET",maxminURL+"?module_1=" + module_1 + "&mark_1=" + mark_1 + "&module_2=" + module_2 + "&mark_2=" + mark_2
            + "&module_3=" + module_3 + "&mark_3=" + mark_3 + "&module_4=" + module_4 + "&mark_4=" + mark_4
            + "&module_5=" + module_5 + "&mark_5=" + mark_5);
        xhttp.send();
        return;
      },

      getSortedModules()
        {
           let sortedModulesURL = 'http://sort.esha.qpc.hal.davecutting.uk/'
           let module_1 = document.getElementById('module_1').value
           let module_2 = document.getElementById('module_2').value
           let module_3 = document.getElementById('module_3').value
           let module_4 = document.getElementById('module_4').value
           let module_5 = document.getElementById('module_5').value

           let mark_1 = document.getElementById('mark_1').value
           let mark_2 = document.getElementById('mark_2').value
           let mark_3 = document.getElementById('mark_3').value
           let mark_4 = document.getElementById('mark_4').value
           let mark_5 = document.getElementById('mark_5').value
            console.log(module_1)
            let xhttp = new XMLHttpRequest();
          xhttp.onreadystatechange = function() {
            if (this.readyState == 4 && this.status == 200) {
              var j = JSON.parse(this.response);
              let sorted_modules_returned = j.sorted_modules;
              let sorted_modules = '';
              for (let i = 0; i < sorted_modules_returned.length; i++) {
                sorted_modules += sorted_modules_returned[i]['module'] + ' - ' + sorted_modules_returned[i]['marks'] + '\r\n';
              }
              document.getElementById('output-text').value = sorted_modules;

            }
          };
           xhttp.open("GET",sortedModulesURL+"?module_1=" + module_1 + "&mark_1=" + mark_1 + "&module_2=" + module_2 + "&mark_2=" + mark_2
           + "&module_3=" + module_3 + "&mark_3=" + mark_3 + "&module_4=" + module_4 + "&mark_4=" + mark_4
           + "&module_5=" + module_5 + "&mark_5=" + mark_5);
           xhttp.send();
           return;
       },
      clearText()
      {
        document.getElementById('module_1').value = '';
        document.getElementById('module_2').value = '';
        document.getElementById('module_3').value = '';
        document.getElementById('module_4').value = '';
        document.getElementById('module_5').value = '';

        document.getElementById('mark_1').value = '';
        document.getElementById('mark_2').value = '';
        document.getElementById('mark_3').value = '';
        document.getElementById('mark_4').value = '';
        document.getElementById('mark_5').value = '';

        document.getElementById('output-text').value = '';
      }


    }

  }
</script>

<template>
<div id="qga">
    <div id="logo">
        QUB GradeMe App
    </div>
    <div>
        <input class="display-module" type="text" id="module_1" name="module_1" placeholder="Module 1">
        <input class="display-mark"  type="text" id="mark_1" name="mark_1" placeholder="Mark 1">
        <br/>
        <input class="display-module" type="text" id="module_2" name="module_2" placeholder="Module 2">
        <input class="display-mark"  type="text" id="mark_2" name="mark_2" placeholder="Mark 2">
        <br/>

        <input class="display-module" type="text" id="module_3" name="module_3" placeholder="Module 3">
        <input class="display-mark"  type="text" id="mark_3" name="mark_3" placeholder="Mark 3">
        <br/>

        <input class="display-module" type="text" id="module_4" name="module_4" placeholder="Module 4">
        <input class="display-mark"  type="text" id="mark_4" name="mark_4" placeholder="Mark 4">
        <br/>

        <input class="display-module" type="text" id="module_5" name="module_5" placeholder="Module 5">
        <input class="display-mark"  type="text" id="mark_5" name="mark_5" placeholder="Mark 5">
        <br/>

    </div>
    <div>
        <textarea class="display-output" id="output-text" rows="5" cols="35" readonly=1 placeholder="Results here..." value="">
        </textarea>
    </div>
    <div>
        <button class="qgabutton-active" @click="Api('getMaxMin');">Highest & Lowest Scoring Modules</button>
    </div>
    <div>
        <button class="qgabutton-active" @click="Api('getSortedModules');">Sort Modules</button>
    </div>
    <div>
        <button class="qgabutton-inactive" @click="Api('getTotalMarks')">Total Marks</button>
    </div>
    <div>
        <button class="qgabutton-inactive" @click="Api('getClassification');">Classify Grade</button>
    </div>
    <div>
        <button class="qgabutton-inactive" @click="Api('getAverage');">Average</button>
    </div>
    <div>
        <button class="qgabutton-inactive" @click="Api('getPassNumber');">Pass </button>
    </div>
    <div>
        <button class="qgabutton-clear" @click="clearText();">Clear</button>
    </div>
 </div>
</template>

<style scoped>
body  {
    font-size: 150%;
    font-family: monospace;
}

#logo
{
    font-family: Calibri, sans-serif;
    font-weight: lighter;
    color: #505050;
    margin: 0.5em;
}

#qga
{
    text-align: center;
    margin-top: 1em;
}

.display-module {
    font-size: 90%;
    color: black;
    background-color: white;
    padding: 0.2em;
    margin: 0.2em;
    font-family: monospace;
    letter-spacing: 0.1em;
    width: 480px;

}

.display-mark{
    font-size: 90%;
    color: black;
    background-color: white;
    padding: 0.2em;
    margin: 0.2em;
    font-family: monospace;
    letter-spacing: 0.1em;
    width: 90px;

}

.display-output {
    font-size: 90%;
    color: white;
    background-color: black;
    padding: 0.2em;
    margin: 0.2em;
    font-family: monospace;
    letter-spacing: 0.1em;
    width: 600px;

}

.qgabutton-active {
    background-color: green;
    color: white;
    padding: 0px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 18px;
    margin: 4px 2px;
    cursor: pointer;
    height: 40px;
    width: 400px;
}

.qgabutton-inactive {
    background-color: gray;
    color: white;
    padding: 0px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 18px;
    margin: 4px 2px;
    cursor: pointer;
    height: 40px;
    width: 400px;
}

.qgabutton-clear {
    background-color: red;
    color: white;
    padding: 0px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 18px;
    margin: 4px 2px;
    cursor: pointer;
    height: 40px;
    width: 400px;
}


</style>
