<style src="./style.css" scoped></style>
<template>
    <div v-show="open" class="vt-dialog__warp" @mousemove.passive="move($event)" @mouseup="moveUp">
        <div class="vt-dialog" :style="pos">
            <div class="vt-dialog__header" v-if="title||closable" @mousedown="moveDown($event)">
                {{title}}
                <svg version="1.1" v-if="closable" class="vt-dialog__close" @click="close()" viewBox="0 0 1024 1024"
                     xmlns="http://www.w3.org/2000/svg"
                     p-id="2455" xmlns:xlink="http://www.w3.org/1999/xlink">
                    <path d="M176.661601 817.172881C168.472798 825.644055 168.701706 839.149636 177.172881 847.338438 185.644056 855.527241 199.149636 855.298332 207.338438 846.827157L826.005105 206.827157C834.193907 198.355983 833.964998 184.850403 825.493824 176.661601 817.02265 168.472798 803.517069 168.701706 795.328267 177.172881L176.661601 817.172881Z"
                          p-id="2456"></path>
                    <path d="M795.328267 846.827157C803.517069 855.298332 817.02265 855.527241 825.493824 847.338438 833.964998 839.149636 834.193907 825.644055 826.005105 817.172881L207.338438 177.172881C199.149636 168.701706 185.644056 168.472798 177.172881 176.661601 168.701706 184.850403 168.472798 198.355983 176.661601 206.827157L795.328267 846.827157Z"
                          p-id="2457"></path>
                </svg>
            </div>
            <div class="vt-dialog__content">
                <slot></slot>
            </div>
            <div class="vt-dialog__footer" v-show="showFooter">
                <slot name="footer"></slot>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        data: function () {
            return {
                open: false,
                closable: false,
                showFooter: true,
                moveEnabled: false,
                pos: {
                    top: '20%',
                    left: '35%'
                },
                mouse: {
                    x: 0,
                    y: 0,
                    top: window.innerHeight * 0.2,
                    left: window.innerWidth * 0.35
                }

            }
        },
        props: {
            title: String,
            opened: {
                type: Boolean,
                default: false
            },
            modal: {
                type: Boolean,
                default: true
            },
            moveable: {
                type: Boolean,
                default: true
            }
        },
        components: {},
        methods: {
            close(){
                this.open = false
                this.$emit("close")
            },
            moveDown(e){
                if (!this.moveable) {
                    return
                }
                this.moveEnabled = true
                this.mouse.x = e.clientX
                this.mouse.y = e.clientY

            },
            moveUp(){
                if (!this.moveable) {
                    return
                }
                this.moveEnabled = false
                if (this.pos.top != "20%") {
                    this.mouse.top = Number.parseInt(this.pos.top)
                    this.mouse.left = Number.parseInt(this.pos.left)
                }

            },
            move(e){
                if (!this.moveable) {
                    return
                }
                if (e.target.className.indexOf("vt-dialog__header") > -1 && this.moveEnabled) {
                    this.pos.top = `${(e.clientY - this.mouse.y + this.mouse.top)}px`
                    this.pos.left = `${(e.clientX - this.mouse.x + this.mouse.left)}px`
                }
            }
        },
        mounted: function () {
            this.open = this.opened
            let closableStr = this.$el.getAttribute('closable')
            this.closable = (closableStr === 'false' ? false : true)
            if (!this.$slots.footer) {
                this.showFooter = false
            }
        },
        watch: {
            opened: function (val) {
                this.open = val
                if (!val) {
                    this.$emit("close")
                }
            }
        }
    }
</script>