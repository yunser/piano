<template>
    <div class="group">
        <button :class="{'white': whites.indexOf(n - 1) > -1, 'black': blacks.indexOf(n - 1) > -1}" 
            v-for="n in 12" 
            :style="{ left: calcLeft(n - 1) + '%' }"
            @click="play(start+n-1)">
            <span class="c" v-show="n === 1">C</span>
            <div class="key" v-if="mode === 'normal'">
                {{ getKey(start+n-1 + 2) }}
            </div>
            <div class="num" v-if="mode === 'normal'">
                <div v-if="group === 4 && getNum(start+n-1 + 2)">.</div>
                {{ getNum(start+n-1 + 2) }}
                <div v-if="group === 2 && getNum(start+n-1 + 2)">.</div>
            </div>
        </button>
    </div>
</template>

<script>
/* eslint-disable */
import {notes} from '../notes.js';
const prefix = 'data:audio/mpeg;base64,';
const base = 2;
const keys = 12;

export default {
    props: {
        mode: {
            type: String,
            default: 'simple'
        },
        group: {
            type: Number,
            default: 0
        }
    },
    data() {
        return {
            // note: changing this line won't causes changes
            // with hot-reload because the reloaded component
            // preserves its current state and we are modifying
            // its initial state.
            blacks: [1, 3, 6, 8, 10],
            whites: [0, 2, 4, 5, 7, 9, 11]
        }
    },
    computed: {
        start() {
            return this.group * keys;
        }
    },
    methods: {
        getKey(code) {
            let keys = {
                26: 'Q',
                28: 'W',
                30: 'E',
                31: 'R',
                33: 'T',
                35: 'Y',
                37: 'U',

                38: 'A',
                40: 'S',
                42: 'D',
                43: 'F',
                45: 'G',
                47: 'H',
                49: 'J',

                50: 'Z',
                52: 'X',
                54: 'C',
                55: 'V',
                57: 'B',
                59: 'N',
                61: 'M',
            }
            return keys[code] || ''
        },
        getNum(code) {
            let keys = {
                26: '1',
                28: '2',
                30: '3',
                31: '4',
                33: '5',
                35: '6',
                37: '7',

                38: '1',
                40: '2',
                42: '3',
                43: '4',
                45: '5',
                47: '6',
                49: '7',

                50: '1',
                52: '2',
                54: '3',
                55: '4',
                57: '5',
                59: '6',
                61: '7',
            }
            return keys[code] || ''
        },
        play(index) {
            console.log('播放', index + base)
            var audio = new Audio(prefix + notes[index + base]);
            audio.play();
        },
        calcLeft(index) {
            if (this.whites.indexOf(index) > -1) {
                return 0
            }
            var unit = 14.29;
            var i = this.blacks.indexOf(index);
            if(i < 2) {
                return unit * (0.75 + i);
            }
            return unit * (1.75 + i);
        },
        click(index) {

        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .group {
        font-size: 0;
        position: relative;
        display: flex;
        flex-grow: 1;
    }
    button {
        /* position: absolute; */
        /* position: relative; */
        width: 14.29%;
        flex: 1;
        height: 300px;
        display: inline-block;
        border: 1px solid #ccc;
        /* border: 1px solid #f00; */
        outline: 0;
        padding: 0;
        box-sizing: border-box;
    }
    .white:active,
    .white.active {
        background: #ececec;
    }
    .white {
        position: relative;
        background: #fff;
    }
    .c {
        position: absolute;
        bottom: 0px;
        left: 0;
        right: 0;
        line-height: 30px;
        text-align: center;
        color: #333;
        font-size: 20px;
    }
    .key {
        position: absolute;
        bottom: 30px;
        left: 0;
        right: 0;
        line-height: 30px;
        text-align: center;
        color: #999;
        font-size: 20px;
    }
    .num {
        position: absolute;
        bottom: 60px;
        left: 0;
        right: 0;
        line-height: 30px;
        text-align: center;
        color: #999;
        font-size: 20px;
    }
    .black {
        position: absolute;
        z-index: 10;
        background: #000;
        border-color: #000;
        height: 150px;
        width: 7.15%;
    }
</style>
