<template>
    <my-page title="钢琴" :page="page">
        <div class="simple-keyboard" v-if="mode === 'simple'">
            <div class="key-row" v-for="row in 3">
                <div class="key" v-for="n in 7" @click="play2(row - 1, n - 1)">
                    <div class="dot dot-top" v-if="row === 1">.</div>
                    <div class="num">{{ n }}</div>
                    <div class="dot" v-if="row === 3">.</div>
                </div>
            </div>
        </div>
        <div class="keyboard" v-if="mode !== 'simple'">
            <group :mode="mode" :group="0" v-if="mode === 'full'"></group>
            <group :mode="mode" :group="1" v-if="mode === 'full'"></group>
            <group :mode="mode" :group="2"></group>
            <group :mode="mode" :group="3"></group>
            <group :mode="mode" :group="4"></group>
            <group :mode="mode" :group="5" v-if="mode === 'full'"></group>
            <group :mode="mode" :group="6" v-if="mode === 'full'"></group>
        </div>
        <ui-drawer class="setting-box" right :open="settingVisible" @close="toggleSetting()">
            <ui-appbar title="设置">
                <ui-icon-button icon="close" @click="toggleSetting" slot="left" />
            </ui-appbar>
            <div class="body">
                <ui-radio class="radio" v-model="mode" label="极简" name="group" nativeValue="simple"/>
                <br>
                <ui-radio class="radio" v-model="mode" label="普通" name="group" nativeValue="normal"/>
                <br>
                <ui-radio class="radio" v-model="mode" label="完整键盘" name="group" nativeValue="full"/>
                <!-- <ui-switch class="switch" v-model="full" label="完整键盘" /> -->
            </div>
        </ui-drawer>
    </my-page>
</template>

<script>
    /* eslint-disable */
    // var attachFastClick = require('fastclick');
    // attachFastClick.attach(document.body);
    import Group from './components/Group'
    const prefix = 'data:audio/mpeg;base64,';
    import {notes} from './notes.js'
    const groupKeys = 12;
    const base = 2;

    export default {
        data () {
            return {
                mode: 'simple',
                settingVisible: false,
                // full: false,
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'settings',
                            click: this.toggleSetting,
                            title: '设置'
                        },
                        {
                            type: 'icon',
                            icon: 'help',
                            to: '/help',
                            title: '帮助'
                        }
                    ]
                }
            }
        },
        mounted() {
            document.body.addEventListener('keydown', this.keyDwon = e => {
                this.play(e.keyCode)
            })
        },
        destroyed() {
            document.body.removeEventListener('keydown', this.keyDwon)
        },
        methods: {
            play2(row, index) {
                let arr = [
                    [50, 52, 54, 55, 57, 59, 61],
                    [38, 40, 42, 43, 45, 47, 49],
                    [26, 28, 30, 31, 33, 35, 37]
                ]
                let audio = new Audio(prefix + notes[arr[row][index]]);
                audio.play();
            },
            play(keyCode) {
                console.log('keyCode', keyCode)
                if (keyCode >= 49 && keyCode <= 55) {
                    let asds = [38, 40, 42, 43, 45, 47, 49]
                    let audio = new Audio(prefix + notes[asds[keyCode - 49]]);
                    audio.play();
                    return
                }
                let keys = [90,88,67,86,66,78,77,65,83,68,70,71,72,74,81,87,69,82,84,89,85];
                if(keys.indexOf(keyCode) < 0) {
                    return;
                }
                let whites = [0, 2, 4, 5, 7, 9, 11];
                let index = base + 2 * groupKeys + whites[keys.indexOf(keyCode) % 7] + parseInt(keys.indexOf(keyCode) / 7)*groupKeys;
                console.log('播放', index)
                let audio = new Audio(prefix + notes[index]);
                audio.play();
            },
            toggleSetting() {
                this.settingVisible = !this.settingVisible
            }
        },
        components: {
            Group
        },
    }
</script>

<style lang="scss" scoped>
.keyboard {
    display: flex;
    // margin: 20px;
}
.control {
    position: absolute;
    top: 16px;
    right: 16px;
    // text-outline: 0 0 #000;
    // text-align: center;
    // margin-top: 50px;
}
.setting-box {
    z-index: 100000;
    .body {
        padding: 16px;
    }
}
.simple-keyboard {
    margin-bottom: 16px;
    .key-row {
        display: flex;
    }
    .key {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        width: 14.29%;
        flex-shrink: 0;
        // line-height: 80px;
        text-align: center;
        height: 80px;
        border: 1px solid #eee;
        cursor: pointer;
        &:active {
            background-color: #ccc;
        }
    }
    .num {
        font-size: 30px;
        line-height: 0.6;
    }
    .dot {
        flex-basis: 4px;
        // height: 4px;
        margin: 4px 0;
        flex-shrink: 0;
        flex-grow: 0;
        line-height: 0px;
        font-size: 30px;
    }
    .dot-top {
        line-height: 23px;
    }
}
</style>
