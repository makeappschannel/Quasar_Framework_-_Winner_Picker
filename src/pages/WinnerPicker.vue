<template>
  <q-page class="flex flex-center bg-black">
    <div class="stage fullscreen"></div>
    <div 
    	:class="{ 'show' : !showWinnerScreen }"
    	class="curtain fullscreen"></div>

    <q-card 
    	v-if="!showWinnerScreen"
    	class="add-names column bg-red">
    	<q-card-section>
    		<h2 class="text-center q-mt-none q-mb-md text-white text-bold deep-shadow">WINNER PICKER</h2>
    		<q-input
		      v-model="namesText"
		      filled
		      type="textarea"
		      bg-color="white"
		      placeholder="Enter names on separate lines."
		      rows="10"
		      cols="40"
		    />
		    <div class="row justify-center q-mt-md">
			    <q-btn
			    	@click="pickWinner"
			    	:disabled="!namesText"
						label="Pick a Winner!"
						color="primary"
						size="xl"
			     />
		    </div>
    	</q-card-section>
    </q-card>
    <div 
    	v-else
    	class="winner-screen">
    	<h1 
    		v-if="pickedWinner"
    		class="text-white absolute-top text-center text-bold deep-shadow">WINNER!!</h1>
    	<div 
    		:class="{ 'zoom' : zoomingNames }"
    		class="name-card">
	    	<q-card
	    		v-if="currentName"
	    		dark
	    		:class="pickedWinner ? 'bg-green-7' : 'bg-primary'"
	    		bordered>
	    		<q-card-section>
	    			<div class="text-h6">{{ currentName }}</div>
	    		</q-card-section>
	    	</q-card>
    	</div>
    	<q-btn
    		v-if="pickedWinner"
    		@click="startAgain"
				color="primary"
				class="start-again absolute"
				label="Start again"
    		 />
    </div>
    <audio
    	v-if="showWinnerScreen"
    	src="/statics/drumroll.mp3"
    	autoplay></audio>
  </q-page>
</template>

<script>
	let animateNamesInterval

	export default {
	  data() {
	  	return {
	  		showWinnerScreen: false,
	  		namesText: '',
	  		namesArray: [],
	  		currentName: '',
	  		pickedWinner: false,
	  		zoomingNames: false
	  	}
	  },
	  methods: {
	  	pickWinner() {
	  		this.showWinnerScreen = true
	  		this.initNamesArray()
	  		this.animateNames()
	  		this.stopAnimatingNames()
	  		this.zoomNames()
	  	},
	  	initNamesArray() {
	  		this.namesArray = this.namesText.split('\n')
	  		this.namesArray = this.shuffle(this.namesArray)
	  		console.log('this.namesArray: ', this.namesArray)
	  	},
	  	animateNames() {
	  		let counter = 0

	  		setTimeout(() => {
		  		animateNamesInterval = setInterval(() => {
	  				this.currentName = ''
		  			if (counter == this.namesArray.length - 1) {
		  				counter = 0
		  			}
		  			else {
		  				counter++
		  			}
		  			setTimeout(() => {
			  			this.currentName = this.namesArray[counter]
		  			}, 100);
		  		}, 200)
	  		}, 700);
	  	},
	  	stopAnimatingNames() {
	  		setTimeout(() => {
	  			clearInterval(animateNamesInterval)
	  			this.pickedWinner = true
	  		}, 8400);
	  	},
	  	zoomNames() {
	  		setTimeout(() => {
	  			this.zoomingNames = true
	  		}, 1000);
	  	},
	  	startAgain() {
	  		this.showWinnerScreen = false
	  		this.pickedWinner = false
	  		this.currentName = ''
	  		this.zoomingNames = false
	  	},
	  	shuffle(a) {
  	    var j, x, i;
  	    for (i = a.length - 1; i > 0; i--) {
  	        j = Math.floor(Math.random() * (i + 1));
  	        x = a[i];
  	        a[i] = a[j];
  	        a[j] = x;
  	    }
  	    return a;
	  	}
	  }
	}
</script>

<style lang="scss">
	.deep-shadow {
		text-shadow: 0 -1px 0 #fff, 0 1px 0 #2e2e2e, 0 2px 0 #2c2c2c, 0 3px 0 #2a2a2a, 0 4px 0 #282828, 0 5px 0 #262626, 0 6px 0 #242424, 0 7px 0 #222, 0 8px 0 #202020, 0 9px 0 #1e1e1e, 0 10px 0 #1c1c1c, 0 11px 0 #1a1a1a, 0 12px 0 #181818, 0 13px 0 #161616, 0 14px 0 #141414, 0 15px 0 #121212, 0 22px 30px rgba(0,0,0,0.9);
	}

	.stage {
		background-image: url(/statics/stage.jpg);
		background-size: cover;
		background-position: center;
		z-index: 1;
		opacity: 0.3;
	}
	.curtain {
		background-image: url(/statics/curtain.png);
		background-size: cover;
		background-position: center;
		z-index: 2;
		transform: translateY(-100%);
		transition: transform 0.3s;
		&.show {
			transform: translateY(0%);
		}
	}
	.add-names {
		z-index: 3;
		.q-textarea {
			textarea {
				font-size: 22px;
				line-height: 1.3em;
			}
		}
	}
	.winner-screen {
		z-index: 4;
		.name-card {
			transform: scale(0.5);
			transition: transform 7.4s linear;
			&.zoom {
				transform: scale(3);
			}
			.q-card {
				box-shadow:
			    0 0 15px 7.5px #fff,
			    0 0 25px 15px #f0f,
			    0 0 35px 22.5px #0ff;
			}
		}
		.start-again {
			bottom: 20px;
			left: 50%;
			transform: translateX(-50%);
		}
	}
</style>
