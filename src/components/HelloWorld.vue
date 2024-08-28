<template>
  <div class="hello">
   
    <p class="result" >{{ input }} <p v-if="showRavno" class="ravno"> &nbsp;=&nbsp; </p>{{ finalresult }}</p>

    <div class="operations">
      <button class="button C" @click="reset">C</button>   
      <button class="button removeLastChar" @click="removeLastChar"> &#8592; </button> 
    </div>

    <div class="numbers">
      <button class="button" v-for="num in numbers" :key="num" @click="appendToInput(num)">{{ num }}</button>
    </div>

    <div class="operations">
      <button class="button oper" v-for="oper in operations" :key="oper" @click="appendToInput(oper)">{{ oper }}</button>
      <button class="button oper btnRavno" @click="getResult">&nbsp;=&nbsp;</button>
    </div>  
    
    <div class="memorys">
        <div v-for="(memory, index) in memorys" :key="index">
        <button class="button memoryUp" @click="addInput(memory)">M{{ index + 1 }} = {{ memory }}</button>
        <button class="button memoryDown" @click="addMemory(index)">add</button>
    </div>
  </div>

    <div class="history">
      <div v-for="item in history" :key="item.id">
        {{ item }}
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      input: '',
      numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0, '.'],
      operations: ['-', '+', '*', '/','(',')', '%', 'корень'],
      finalresult: null,
      showRavno: false,
      memorys: [null, null, null],
      resultObtained: false,
      history: [],
    }
  },
  methods: {
    appendToInput(value) {
      if (this.finalresult === "Ошибка") {
        this.input = '';
        this.finalresult = '';
      }  

      const lastElem = this.input[this.input.length - 1];
      if (this.resultObtained) {
        if (!isNaN(parseFloat(value)) && isFinite(value)) {
          // Если вводится число после результата
          this.input = value;
          this.resultObtained = false;
          this.finalresult = null;
          this.showRavno = false;
        } else {
          // Если вводится оператор после результата
          if (!isNaN(Number(lastElem))) {
            this.input = String(this.finalresult) + value;
          } else if (['+', '-', '*', '/'].includes(lastElem)) {
            // Последний элемент — оператор
            this.input = this.input.slice(0, -1) + value;
          }
          this.resultObtained = false;
          this.showRavno = false;
          this.finalresult = null;
        }
      } else {
        if (['+', '-', '*', '/'].includes(value) && ['+', '-', '*', '/'].includes(lastElem)) {
          this.input = this.input.slice(0, -1) + value;
        } else {
          this.input += value;
        }
      }
    },

    getResult() {
      try {      
        let result = eval(this.input);
        this.finalresult = parseFloat(result.toFixed(10));
        this.showRavno = true;
        this.resultObtained = true;
        this.history.push(this.input + ' ' + '=' + ' ' + this.finalresult);
      } catch (e) {
        this.finalresult = 'Ошибка';
        this.input = '';
      }
    },
    reset(){
      this.input = '';
      this.finalresult = null;
      this.showRavno = false;
      this.resultObtained = false;
    },
    addMemory(index){
      if(this.finalresult){
        this.memorys[index] = this.finalresult;
      }else{
        let valueToStore = parseFloat(this.input);
        if (!isNaN(valueToStore) && isFinite(valueToStore)) {
          this.memorys[index] = valueToStore;      
        }
      }      
    },
    addInput(value){
        if (value !== null) {
        this.input += value;
      }
     
    },
    removeLastChar(){
      if(!this.finalresult){
        this.input = this.input.slice(0, -1);
      }
    }
  },
}
</script>

<style scoped>

.hello {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.numbers{
  display: grid;
  grid-template-columns: repeat(3, 150px); 
  gap: 10px; 
  justify-items: center; 
  align-items: center;
  justify-content: center;
  margin-top: 10px;
  margin-bottom: 16px;
}

.operations{
  display: grid;
  grid-template-columns: repeat(3, 150px); 
  gap: 10px; 
  justify-items: center; 
  align-items: center;
  justify-content: center;  
  margin-bottom: 16px;
}

.button {
  width: 150px;
  height: 30px;
  border-radius: 4px;
  border: 1px solid;
  font-weight: bold;  
}

.result{
  width: 300px;
  min-height: 90px;
  display: flex; 
  justify-content: center;
  font-size: 40px;
  flex-wrap: wrap;
}
.ravno{
  margin: 0;
}
.btnRavno{
  grid-column: 3 / 4;
  grid-row: 1/3;
  height: calc(30px * 2 + 10px);
}
.memorys{
  display: grid;
  grid-template-columns: repeat(3, 150px); 
  gap: 10px; 
  justify-items: center; 
  align-items: center;
  justify-content: center; 
}
.C{
  background-color: orange;
}
.removeLastChar{
  background-color: rgba(238, 201, 19, 0.963);
}
.oper{
  background-color: rgba(195, 222, 237, 0.963);
}
.memoryUp{
  border-radius: 4px 4px 0 0;
  border-bottom: .5px;
  background-color: rgb(163, 214, 197);
}
.memoryDown{
  border-radius: 0 0 4px 4px;
  background-color: rgb(185, 212, 203);
}
.history{
  margin-top: 16px;
  color: gray;
  font-size: 20px;
}
</style>