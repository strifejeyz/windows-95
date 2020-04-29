<template>
    <div id="taskbar">
        <div class="start-menu">
            <transition name="popup">
                <div class="start-menu-list" v-if="startMenu">
                    <div class="side-brand"><img src="/assets/img/side-brand.png" class="img-fluid"></div>
                    <div class="menu-items">
                        <button @click="restore('intro')">
                            <img src="/assets/img/icons/folder-open.png">
                            <span class="menu-name">Intro</span>
                        </button>
                        <button @click="restore('projects')">
                            <img src="/assets/img/icons/computer.png">
                            <span class="menu-name">My Projects</span>
                        </button>
                        <button @click="restore('strife')">
                            <img src="/assets/img/icons/strife.png">
                            <span class="menu-name">Strife Framework</span>
                        </button>
                        <button @click="restore('skills')">
                            <img src="/assets/img/icons/skills.png">
                            <span class="menu-name">Skills</span>
                        </button>
                        <a href="http://github.com/strifejeyz" target="_blank">
                            <img src="/assets/img/icons/git.png">
                            <span class="menu-name">Github</span>
                        </a>
                        <span class="divider"></span>
                    </div>
                </div>
            </transition>
            <div class="flex flex-left">
                <button class="start-button" @click="startMenu = !startMenu">
                    <img src="/assets/img/icons/start-button.png"> <span class="start-word">Start</span>
                </button>
                <div class="minimized-items">
                    <transition name="slideLeft">
                        <button v-if="folders.intro.visible" class="border" @click="restore('intro', folders.intro.id)">
                            <img src="/assets/img/icons/folder-open.png"> <span>Intro</span>
                        </button>
                    </transition>

                    <transition name="slideLeft">
                        <button v-if="folders.projects.visible" class="border" @click="restore('projects', folders.projects.id)">
                            <img src="/assets/img/icons/computer.png"> <span>My Projects</span>
                        </button>
                    </transition>

                    <transition name="slideLeft">
                        <button v-if="folders.strife.visible" class="border" @click="restore('strife', folders.strife.id)">
                            <img src="/assets/img/icons/strife.png"> <span>Strife</span>
                        </button>
                    </transition>

                    <transition name="slideLeft">
                        <button v-if="folders.skills.visible" class="border" @click="restore('skills', folders.skills.id)">
                            <img src="/assets/img/icons/skills.png"> <span>Skills</span>
                        </button>
                    </transition>

                    <transition name="slideLeft">
                        <button v-if="folders.trash.visible" class="border" @click="restore('trash', folders.trash.id)">
                            <img src="/assets/img/icons/trash.png"> <span>Trash</span>
                        </button>
                    </transition>
                </div>
            </div>
        </div>

        <div class="notif">
            <span class="time">{{time}}</span>
        </div>
    </div>
</template>

<script>
    import Axios from "axios";
    export default {
        props: ['folders'],
        data: function () {
            return {
                startMenu: false,
                time: null
            }
        },
        mounted () {
            let self = this;
            Axios.post("/time").then(function (r) {
                self.time = r.data;
            });

            window.addEventListener("click", function (e) {
                Axios.post("/time").then(function (r) {
                    self.time = r.data;
                });
            });
        },
        
        methods: {
            restore: function (folderName, id) {
                this.startMenu = false;
                this.$emit("restore", folderName);

                let topLayer    = document.querySelector("#" + id);
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
        }
    }
</script>

<style scoped>

    #taskbar {
        width: 100%;
        background: #C0C0C0;
        border: 3px solid #eaeaea;
        position: fixed;
        bottom: 0;
        height: 40px;
        display: flex;
        justify-content: space-between;
        z-index: 500;
        padding: 1px;
    }

    #taskbar .minimized-items {
        position: relative;
        left: 5px;
    }

    #taskbar .minimized-items button {
        padding: 4px !important;
        padding-left: 10px !important;
        width: 150px;
        position: relative;
        display: inline-table;
        text-align: left;
        background: #C0C0C0;
        outline: none
    }

    #taskbar .minimized-items button:focus {
        background: #a1a1a1;
    }

    #taskbar .minimized-items img {
        height: 20px !important;
        position: relative;
        top: -3px;
        margin-right: 5px;
    }

    #taskbar .start-button {
        background: none;
        border: 2px solid #ffffff;
        border-bottom: 2px solid #060606;
        border-right: 2px solid #060606;
        outline: none;
        font-weight: 600;
        position: relative;
        padding: 0 10px;
        height: 34px;
    }

    #taskbar .start-button .start-word {
        color: #000000 !important;
    }
    #taskbar .start-button img {
        padding: 0px 0;
        position: relative;
        top: -1px;
        right: 1px;

    }

    #taskbar .notif {
        border: 2px solid #8E8E8E;
        border-bottom: 2px solid #e0e0e0;
        border-right: 2px solid #e0e0e0;
        padding: 0 10px;
    }

    #taskbar .notif .time {
        position: relative;
        font-family: monospace;
        font-size: 14px;
        top: 4px;
        font-weight: 600
    }

    #taskbar .start-menu-list {
        position: fixed;
        z-index: 201;
        height: 300px;
        background: #c0c0c0;
        bottom: 40px;
        left: 1px;
        border: 3px solid #eaeaea;
        border-bottom: 3px solid #717171;
        border-right: 3px solid #717171;
        display: flex;
        justify-content: left;
    }

    #taskbar .side-brand {
        font-weight: bold;
        letter-spacing: 1px;
    }

    #taskbar .side-brand img {
        width: 100%;
        height: 100%;
    }
    #taskbar .menu-items a,
    #taskbar .menu-items button {
        display: block;
        width: 180px;
        background: none;
        border: none;
        text-align: left;
        padding: 5px;
        padding-left: 10px;
        text-decoration: none;
        color: #333333
    }
    #taskbar .menu-items a:hover,
    #taskbar .menu-items button:hover {
        background: #e4e4e4
    }
    #taskbar .menu-items .menu-name {
        position: relative;
        left: 5px;
    }
    #taskbar .menu-items .divider {
        border-top: 1px solid #989898;
        border-bottom: 2px solid #dadada;
        display: block;
        margin-top: 10px;
        margin-bottom: 10px;
    }


    .slideLeft-enter-active {
        animation: ease-in .5s;
    }

    .slideLeft-leave-active {
        animation: ease-in .3s reverse;
    }

    @keyframes ease-in {
        0% {
            opacity: 0;
            width: 0;
        }

        50% {
            opacity: 0.8;
            width: 180px;
        }

        100% {
            opacity: 1;
            width: 150px;
        }
    }


    .popup-enter-active {
        animation: showUp .3s;
    }

    .popup-leave-active {
        animation: showUp .3s reverse;
    }


    @keyframes showUp {
        0% {
            height: 0;
            opacity: 0;
        }

        20% {
            opacity: 1;
        }


        80% {
            height: 320px;
        }

        100% {
            height: 300px;
        }
    }
</style>
