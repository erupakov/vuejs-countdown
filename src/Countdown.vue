<template>
    <ul class="vue-countdown">
        <li>
            <p class="digit">{{ days | twoDigits }}</p>
            <p class="text">days</p>
        </li>
        <li>
            <p class="digit">{{ hours | twoDigits }}</p>
            <p class="text">hours</p>
        </li>
        <li>
            <p class="digit">{{ minutes | twoDigits }}</p>
            <p class="text">Min</p>
        </li>
        <li>
            <p class="digit">{{ seconds | twoDigits }}</p>
            <p class="text">Sec</p>
        </li>
    </ul>
</template>

<script>
import Vue from 'vue'

let interval = null;

export default {
    props: ['deadline', 'stop'],
    data() {
        return {
            now: Math.trunc((new Date()).getTime() / 1000),
            diff: 0
        }
    },
    mounted() {
        interval = setInterval(() => {
            this.now = Math.trunc((new Date()).getTime() / 1000)
        }, 1000)

        console.log(interval)
    },
    computed: {
	date() {
	    console.log( 'Updated: ' + this.deadline )
	    return Math.trunc(Date.parse(this.deadline) / 1000)
	},

        seconds() {
            return Math.trunc(this.diff) % 60
        },

        minutes() {
            return Math.trunc(this.diff / 60) % 60
        },

        hours() {
            return Math.trunc(this.diff / 60 / 60) % 24
        },

        days() {
            return Math.trunc(this.diff / 60 / 60 / 24)
        }
    },
    watch: {
        now(value){
            this.diff = this.date - this.now;
            if(this.diff <= 0 || this.stop){
                this.diff = 0;
                // Remove interval
                clearInterval(interval);
            }
        }
    },
    filters: {
        twoDigits: function (value) {
	    if ( value.toString().length <= 1 ) {
	        return '0'+value.toString()
	    }
	    return value.toString()
        }
    }
}
</script>
