<template>
    <div class="dragresize" :id="id">
        <slot></slot>
        <div class="resizer nw"></div>
        <div class="resizer ne"></div>
        <div class="resizer se"></div>
        <div class="resizer sw"></div>
    </div>
</template>

<script>
    export default {
        name: "draggable",
        props: ['id', "pins"],

        mounted() {
            const el = document.querySelector("#" + this.id);
            let isResizing = false;
            el.addEventListener("mousedown", mousedown);

            function mousedown(e) {
                window.addEventListener("mousemove", mousemove);
                window.addEventListener("mouseup", mouseup);

                let prevX = e.clientX;
                let prevY = e.clientY;

                function mousemove(e) {
                    if (!isResizing) {
                        let newX = prevX - e.clientX;
                        let newY = prevY - e.clientY;

                        const rect = el.getBoundingClientRect();

                        el.style.left = rect.left - newX + "px";
                        el.style.top = rect.top - newY + "px";

                        prevX = e.clientX;
                        prevY = e.clientY;
                    }
                }

                function mouseup() {
                    window.removeEventListener("mousemove", mousemove);
                    window.removeEventListener("mouseup", mouseup);
                }
            }

            const resizers = document.querySelectorAll(".resizer");
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
                    window.addEventListener("mouseup", mouseup);

                    function mousemove(e) {
                        const rect = el.getBoundingClientRect();

                        if (currentResizer.classList.contains("se")) {
                            el.style.width = rect.width - (prevX - e.clientX) + "px";
                            el.style.height = rect.height - (prevY - e.clientY) + "px";
                        } else if (currentResizer.classList.contains("sw")) {
                            el.style.width = rect.width + (prevX - e.clientX) + "px";
                            el.style.height = rect.height - (prevY - e.clientY) + "px";
                            el.style.left = rect.left - (prevX - e.clientX) + "px";
                        } else if (currentResizer.classList.contains("ne")) {
                            el.style.width = rect.width - (prevX - e.clientX) + "px";
                            el.style.height = rect.height + (prevY - e.clientY) + "px";
                            el.style.top = rect.top - (prevY - e.clientY) + "px";
                        } else {
                            el.style.width = rect.width + (prevX - e.clientX) + "px";
                            el.style.height = rect.height + (prevY - e.clientY) + "px";
                            el.style.top = rect.top - (prevY - e.clientY) + "px";
                            el.style.left = rect.left - (prevX - e.clientX) + "px";
                        }

                        prevX = e.clientX;
                        prevY = e.clientY;
                    }

                    function mouseup() {
                        window.removeEventListener("mousemove", mousemove);
                        window.removeEventListener("mouseup", mouseup);
                        isResizing = false;
                    }
                }
            }
        }
    }
</script>

<style scoped>
    .dragresize {
        position: absolute;
        z-index: 400;
    }


    .dragresize .resizer {
        position: absolute;
        width: 10px;
        height: 10px;
    }

    .dragresize .resizer.nw {
        top: 0;
        left: 0;
        cursor: nw-resize;
    }

    .dragresize .resizer.ne {
        top: 0;
        right: 0;
        cursor: ne-resize;
    }

    .dragresize .resizer.se {
        bottom: -1px;
        right: 0;
        cursor: se-resize;
    }

    .dragresize .resizer.sw {
        bottom: 0;
        left: 0;
        cursor: sw-resize;
    }
</style>