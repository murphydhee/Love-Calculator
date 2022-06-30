<script>
export default {
  data() {
    return {
        firstName: '',
        secondName: '',
        message: '',
        historys: '',
        result: [],
    }
  },
  methods: {
    onSubmit(){
      let checker =  JSON.parse(localStorage.getItem('resultsArray'));
      if (!this.firstName && !this.secondName) {
        alert('Please fill both boxes to get accurate results');
      } else if (!this.firstName){
        alert("Please fill lady's name")
      } else if (!this.secondName){
        alert("Please fill guys's name")
      }  else if (checker == null || checker == [] || checker == '') {
      // if local storage is empty, runs api function
        this.runAPI()
      } else {
      // if local storage is not empty, runs function to search 
        this.checkAPI()
      }
    },
    runAPI() {
       let URL = `https://loverapi.herokuapp.com/api/v1/calculate?personA=${this.firstName}&personB=${this.secondName}`
      fetch(URL)
        .then(response => response.json())
        .then(data => {
          const newData = data;

          // create an object for each search
          const personsData = new Object();
          personsData.personA = this.firstName;
          personsData.personB = this.secondName;
          personsData.msg = newData.message;
          personsData.no = newData.result;
          this.result.push(personsData);
          this.message = newData;

          //pushes to localstorage
          localStorage.setItem('resultsArray', JSON.stringify(this.result))
          });
    },
    checkAPI() {
      // gets stored data from localstorage 
         this.historys = JSON.parse(localStorage.getItem('resultsArray'));
         alert('Loading result...please wait')
         let resultHistory = this.historys;

        // filters localStorage and searches inputed value
         function filterArray (array, key1, value1, key2, value2) {
             return array.filter(function(e) {
             return e[key1] == value1 && e[key2] == value2;
          });
         };

        let theOne = filterArray(resultHistory, 'personA', `${this.firstName}`, 'personB', `${this.secondName}`)

         if (theOne.length == 0){

          //runs api if data is not there
          alert('not found')
          this.runAPI();
         } else {

          //displays if data exists
          alert('Previous search result found')
          console.log(theOne)
          this.message = theOne[0];
          this.message.message = theOne[0].msg;
          this.message.result = theOne[0].no;
         }
    },
    reset() {
        this.firstName = '';
        this.secondName = '';
        this.result = '';
    }
  },
} 
</script>

<template>
  <section>
    <h1>Love calculator</h1>
    <h2>Fill in the your name and the name of your partner and know your love rate!</h2>
    <form action="" @submit.prevent="onSubmit">
      <div>
        <label for="first name">Name of Lady/woman is </label>
        <input 
          type="text"
          id="first name"
          name="first name"
          v-model="firstName"
        >
      </div>
      <div>
        <label for="second name">Name of Gentleman/Man is </label>
        <input 
          type="text"
          id="second name"
          name="second name"
          v-model="secondName"
        >
      </div>
      <div>
        <button type="submit" value="Submit" @click="onSubmit">Calculate</button>
      </div>
      <div>
        <button type="button" value="Resset" @click="reset">Reset</button>
      </div>
      <div>
        <h1>Result:</h1>
        <h2><span>Love Percentage: </span>{{this.message.result}}</h2>
        <h2><span>Message: </span>{{this.message.message}}</h2>
      </div>
    </form>
  </section>
</template>
