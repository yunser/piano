<template>
    <my-page title="关于" backable>
        <div>钢琴 v1.0.0</div>
        <button class="set-full-screen" @click="fullScreen">全屏</button>
        <button class="print" @click="print">打印</button>
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
            }
        },
        mounted() {
            // this.$http.get('/version').then(
            //     response => {
            //         let data = response.data
            //         if (data.code === 200) {
            //             console.log(data)
            //             this.version = data.data
            //         }
            //     },
            //     response => {
            //         console.log(response)
            //     })
//            this.$http.post('/example', this.$qs.stringify({
//                param1: 'value1',
//                param2: 'value2'
//            })).then(
//                response => {
//                    let data = response.data
//                    if (data.code === 200) {
//                        console.log(data.data)
//                    }
//                },
//                response => {
//                    console.log(response)
//                })
        },
        methods: {
            fullScreen() {
                console.log('全屏')
                // callEvent('SET_FULL_SCREEN')
                console.log(window._app)
                if (!window._app) {
                    this.$message({
                        text: '浏览器端不支持此功能'
                    })
                    return
                }
                window._app.fullScreen()
            },
            print() {
                console.log('打印')
                // callEvent('ASYNC_EVENT', {word: 'hello world'}).then(() => {
                //     console.log('done')
                // })
            }
        }
    }
</script>

<style scoped>
    .asd {
    }
</style>
