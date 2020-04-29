<template>
    <div class="draggable" :id="id">
        <slot></slot>
    </div>
</template>

<script>
    export default {
        name: "draggable",
        props: ['id'],

        mounted() {
            let el = document.querySelector("#" + this.id);
            el.addEventListener("mousedown", mousedown);
            function mousedown(e) {
                let prevX = e.clientX;
                let prevY = e.clientY;
                window.addEventListener("mousemove", mousemove);
                window.addEventListener("mouseup", mouseup);
                function mousemove (e){
                    let   newX = prevX - e.clientX;
                    let   newY = prevY - e.clientY;
                    const rect = el.getBoundingClientRect();

                    el.style.left = rect.left - newX + "px";
                    el.style.top  = rect.top  - newY + "px";

                    prevX = e.clientX;
                    prevY = e.clientY;
                }
                function mouseup (){
                    window.removeEventListener("mousemove", mousemove);
                    window.removeEventListener("mouseup", mouseup);
                }
            }
        }
    }
</script>

<style scoped>
    .draggable { position: absolute }
</style>