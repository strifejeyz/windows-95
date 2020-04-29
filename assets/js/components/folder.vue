<template>
    <div class="folder" :id="data.id" @click="reIndex" @mousedown="reIndex" @mouseup="reIndex">
        <div>
            <div class="folder-head">
                <div class="heading">
                    <img :src="data.icon" class="img-fluid file-icon">
                    <span class="folder-name">
                        {{data.title}}
                    </span>
                </div>
                <div class="buttons">
                    <img draggable="false" src="/assets/img/folder/minimize.png" @click="minimize"/>
                    <img draggable="false" src="/assets/img/folder/maximize.png" @click="maximize"/>
                    <img draggable="false" src="/assets/img/folder/close.png" @click="close"/>
                </div>
            </div>
            <ul class="menu">
                <li><u>F</u>ile</li>
                <li><u>E</u>dit</li>
                <li><u>H</u>elp</li>
            </ul>
            <div class="folder-body">
                <slot></slot>
                <div class="resizer se"></div>
                <div class="resizer sw"></div>
            </div>
            <div class="folder-foot">
                <div style="display:flex;justify-content:space-between">
                    <div>{{data.items}} items</div>
                    <div><img :src="data.icon" class="img-fluid">{{data.title}}</div>
                </div>
            </div>
        </div>

        <div class="resizer nw"></div>
        <div class="resizer ne"></div>
        <div class="resizer se"></div>
        <div class="resizer sw"></div>
    </div>
</template>

<script>

    export default {
        props: ['data'],
        data: function () {
            return {
                pins: false,
                box: {
                    height: null,
                    width:  null,
                    top:    null,
                    left:   null,
                },
                full: false,
            }
        },
        methods: {
            close:    function () {
                this.$emit("close");
            },
            maximize: function () {
                if (this.full) {
                    return this.default();
                }

                const folder    = document.querySelector("#" + this.data.id);
                const body      = document.querySelector("#" + this.data.id + " .folder-body");
                this.box.height = getComputedStyle(folder).height;
                this.box.width  = getComputedStyle(folder).width;
                this.box.left   = getComputedStyle(folder).left;
                this.box.top    = getComputedStyle(folder).top;

                folder.style.width = "100%";
                folder.style.height = (window.innerHeight - 40) + "px";
                folder.style.left = "0";
                folder.style.top = "0";

                body.style.width = "100%";
                body.style.height = (window.innerHeight - 40) + "px";
                body.style.left = "0";
                body.style.top = "0";

                this.full = true;
            },
            minimize: function () {
                this.$emit("minimize");
            },
            default:  function () {
                const folder = document.querySelector("#" + this.data.id);
                const body = document.querySelector("#" + this.data.id + " .folder-body");

                folder.style.height = "auto";
                folder.style.width  = "auto";
                folder.style.left   = this.box.left;
                folder.style.top    = this.box.top;

                body.style.height = this.box.height;
                body.style.width  = this.box.width;
                    body.style.left   = "unset";
                body.style.top    = "unset";

                this.box = {height:null,width:null,top:null,left:null}
                this.full = false;
            },
            reIndex: function () {
                let topLayer    = document.querySelector("#" + this.data.id);
                let bottomLayer = document.querySelectorAll(".folder");

                for(let i = 0; i < bottomLayer.length; i++) {
                    if (bottomLayer[i]) {
                        bottomLayer[i].style.zIndex = (200 + i);
                    }
                }
                if (topLayer) {
                    topLayer.style.zIndex = "220";
                }
            }
        },
        mounted() {
            const el       = document.querySelector("#" + this.data.id);
            const elHead   = document.querySelector("#" + this.data.id + " .folder-head");
            const elBody   = document.querySelector("#" + this.data.id + " .folder-body");
            let isResizing = false;
            elHead.addEventListener("mousedown", mousedown);

            function mousedown(e) {
                window.addEventListener("mousemove", mousemove);
                window.addEventListener("mouseup",   mouseup);

                let prevX = e.clientX;
                let prevY = e.clientY;

                function mousemove(e) {
                    if (!isResizing) {
                        let newX      = prevX - e.clientX;
                        let newY      = prevY - e.clientY;
                        const rect    = el.getBoundingClientRect();
                        el.style.left = rect.left - newX + "px";
                        el.style.top  = rect.top - newY + "px";
                        prevX         = e.clientX;
                        prevY         = e.clientY;
                    }
                }

                function mouseup() {
                    window.removeEventListener("mousemove", mousemove);
                    window.removeEventListener("mouseup",   mouseup);
                }
            }

            const resizers = document.querySelectorAll("#" + this.data.id + " .resizer");
            let currentResizer;

            if (this.pins == false) {
                resizers[0].style.background = "transparent";
                resizers[1].style.background = "transparent";
                resizers[2].style.background = "transparent";
                resizers[3].style.background = "transparent";
            } else {
                resizers[0].style.background = "#ffffff";
                resizers[1].style.background = "#ffffff";
                resizers[2].style.background = "#ffffff";
                resizers[3].style.background = "#ffffff";
            }

            for (let resizer of resizers) {
                resizer.addEventListener("mousedown", mousedown);

                function mousedown(e) {
                    currentResizer = e.target;
                    isResizing = true;

                    let prevX = e.clientX;
                    let prevY = e.clientY;

                    window.addEventListener("mousemove", mousemove);
                    window.addEventListener("mouseup",   mouseup);

                    function mousemove(e) {
                        const rect = elBody.getBoundingClientRect();

                        if (currentResizer.classList.contains("se")) {
                            elBody.style.width = rect.width - (prevX - e.clientX) + "px";
                            elBody.style.height = rect.height - (prevY - e.clientY) + "px";
                        } else if (currentResizer.classList.contains("sw")) {
                            elBody.style.width = rect.width + (prevX - e.clientX) + "px";
                            elBody.style.height = rect.height - (prevY - e.clientY) + "px";
                            elBody.style.left = rect.left - (prevX - e.clientX) + "px";
                        } else if (currentResizer.classList.contains("ne")) {
                            elBody.style.width = rect.width - (prevX - e.clientX) + "px";
                            elBody.style.height = rect.height + (prevY - e.clientY) + "px";
                            elBody.style.top = rect.top - (prevY - e.clientY) + "px";
                        } else {
                            elBody.style.width = rect.width + (prevX - e.clientX) + "px";
                            elBody.style.height = rect.height + (prevY - e.clientY) + "px";
                            elBody.style.top = rect.top - (prevY - e.clientY) + "px";
                            elBody.style.left = rect.left - (prevX - e.clientX) + "px";
                        }

                        prevX = e.clientX;
                        prevY = e.clientY;
                    }

                    function mouseup() {
                        window.removeEventListener("mousemove", mousemove);
                        window.removeEventListener("mouseup",   mouseup);
                        isResizing = false;
                    }
                }
            }
        }
    }
</script>

<style scoped>
    .folder {
        position: absolute;;
        z-index: 300;
    }


    .folder .folder-head .file-icon {
        width: 20px;
        margin-right: 4px;
    }

    .folder .folder-head .folder-name {
        position: relative;
        top: 1px;
    }

    .folder .menu{
        list-style: none;
        margin: 0;
        padding: 0;
        width: 100%;
        background: silver;
    }

    .folder .menu li {
        display: inline-table;
        padding: 3px 7px;
    }

    .folder .folder-body {
        border: 2px solid #585858;
        border-right: 2px solid #ffffff;
        border-bottom: 2px solid #ffffff;
    }
    .folder-foot {
        border: 2px solid #999999 !important;
        border-bottom: 2px solid #ffffff !important;
        border-right: 2px solid #ffffff !important;
        background: #c0c0c0;
        padding: 0 3px;
        font-size: 12px
    }

    .folder-foot img {
        height: 15px;
        position: relative;
        right: 5px;
        top: -1px;
    }

    .folder {
        background: #C3C3C3;
        height: auto;
        border: 3px solid #c7c7c7;
        border-bottom: 2px solid #2f2f2f;
        border-right: 2px solid #2f2f2f;
        margin-left: auto;
        margin-right: auto;
        outline: none;
        user-select: none;
        vertical-align: middle
    }

    .folder .folder-head {
        background: #000082;
        vertical-align: middle;
        color: #e1e1e1;
        padding: 2px 4px;
        display: flex;
        justify-content: space-between;
    }

    .folder .folder-head .buttons img {
        width: 22px;
        height: 20px;
        position: relative;
        background: #c0c0c0;
        opacity: 1;
        border: 1px solid #ffffff;
        border-bottom: 1px solid #000;
        border-right: 1px solid #000;
        right: -1px;
    }

    .folder .folder-head .buttons img:hover {
        background: #969696;
        opacity: 1;
        cursor: pointer;
    }

    .folder .folder-body img {
    }

    .folder .folder-body {
        text-align: center !important;
        vertical-align: middle;
        overflow: auto;
        min-height: 100px;
        min-width: 200px;
    }

    .folder .folder-foot {

    }
    .resizer {
        position: absolute;
        width: 10px;
        height: 10px;
        background: transparent;
    }

    .resizer.nw {
        top: 0;
        left: 0;
        cursor: nw-resize;
    }

    .resizer.ne {
        top: 0;
        right: 0;
        cursor: ne-resize;
    }

    .resizer.se {
        bottom: -1px;
        right: 0;
        cursor: se-resize;
    }

    .resizer.sw {
        bottom: 0;
        left: 0;
        cursor: sw-resize;
    }
</style>