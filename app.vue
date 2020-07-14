console.log(' e '.match(/([\s]+)/g).length)

var mytyperacer = {
	data: function () {
    return {
      sentence: '',
      corrsen: 'Typing this text will show you how fast you can type!',
      corr: true,
      tid1: 0,
      tidtag: 0,
      exColor: ''
      }
  },
  props: {
		wpm: ''
  },  
  template: `
  	<div>
    <hr>
    <p>{{ corrsen }}</p>
    <p>{{ tidtag }} wpm</p>
    <input type="text" v-model="sentence">
    <p :style="{color: exColor}">{{ sentence }}</p>
    <hr>
    <button @click="loadXMLDoc">Get words!</button>

    <p class="text" v-if="sentence==corrsen">I did it!, {{ tidtag }} wpm</p>
    </div>
  	`,
  watch: {
  	sentence:function() {
    	if (this.sentence.length > 0 && this.corr) {
      this.corr = false
      var date = new Date();
      this.tid1 = date.getTime()
      } else {
      var date2 = new Date();
      try {
      	this.tidtag = Math.round(((this.sentence.length - this.sentence.match(/([\s]+)/g).length) /  5) / ((date2.getTime() - this.tid1) / 1000 / 60)) 
      } catch (err) {}
      }
      if (this.sentence == this.corrsen.substring(0,(this.sentence.length))) {
      	this.exColor = 'green'
      } else {
      	this.exColor = 'red'
      }
    }
  },
  methods: {
    loadXMLDoc: function () {
        var vm = this
        var xhttp = new XMLHttpRequest();
        xhttp.onreadystatechange = function() {
          if (this.readyState == 4 && this.status == 200) {
            vm.corrsenList = JSON.parse(this.responseText);
            vm.corrsen = vm.corrsenList.join(' ')
            vm.sentence = ''
            vm.corr = true
            vm.wpm = vm.tidtag
            
          }
        };
        xhttp.open("GET", "https://random-word-api.herokuapp.com/word?number=10", true);
        xhttp.send();
        
      }
  }
}



new Vue({
  el: "#app",
  data: {
  
  },
  components: {
  	'my-type-racer': mytyperacer
  }
})

console.log(document.querySelector('.main').innerHTML)
