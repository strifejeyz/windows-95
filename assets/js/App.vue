<template>
    <div class="app">
        <div class="desktop">
            <!-- FOLDERS -->
            <draggable :id="'icon-intro'">
                <icon :data="icons.intro" @openApp="folders.intro.visible=true" class="nm8-moveFromTop"/>
            </draggable>

            <draggable :id="'icon-projects'" >
                <icon :data="icons.projects" @openApp="folders.projects.visible=true" class="nm8-moveFromTop nm8-delay100"/>
            </draggable>

            <draggable :id="'icon-strife'">
                <icon :data="icons.strife" @openApp="folders.strife.visible=true" class="nm8-moveFromTop nm8-delay200"/>
            </draggable>

            <draggable :id="'icon-skills'">
                <icon :data="icons.skills" @openApp="folders.skills.visible=true" class="nm8-moveFromTop nm8-delay300"/>
            </draggable>

            <draggable :id="'icon-git'">
                <span @dblclick="goToGithub()">
                    <icon :data="icons.git" class="nm8-moveFromTop nm8-delay400"/>
                </span>
            </draggable>

            <draggable :id="'icon-trash'">
                <icon :data="icons.trash" @openApp="folders.trash.visible=true" class="nm8-moveFromRight nm8-delay500"/>
            </draggable>

            <!-- FOLDERS -->
            <transition name="bounce">
                <folder :data     = "folders.intro"
                        @close    = "folders.intro.visible=false"
                        @minimize = "minimize('intro')"
                        v-show    = "!folders.intro.minimized && folders.intro.visible"
                        v-if      = "folders.intro.visible">
                    <div style="padding: 20px;">
                        <h1>Windows 95 Experience</h1>
                        <p>Let's bring you back to 1995!</p>
                        <p>
                            Windows 95 is a consumer-oriented operating system developed by Microsoft as part of its Windows 9x
                            family of operating systems. The first operating system in the 9x family, it is the successor to
                            Windows 3.1x, and was released to manufacturing on August 15, 1995, and generally to retail on
                            August 24, 1995.<br><br>
                            Accompanied by an extensive marketing campaign, Windows 95 introduced numerous functions and features that
                            were featured in later Windows versions, such as the taskbar, the "Start" button and the ways the user could navigate.
                            <br>
                        </p>
                        <h4>Here's the list of things you can do here:</h4>
                        <div class="intro-list-of-stuff">
                            <ul>
                                <li>Drag and drop icons/folders</li>
                                <li>Resize folders</li>
                                <li>Open icons on the desktop/start menu</li>
                                <li>Minimize/Maximize folders</li>
                                <li>Interact with buttons</li>
                                <li>And of course, read something.</li>
                                <li>View it in <u><b><a href="javascript:;" @click="fullscreen()">Fullscreen</a></b></u></li>
                            </ul>
                        </div>
                        <hr>
                        <a target="_blank" href="https://facebook.com/jeyzstrife">&copy; Jesse Canonigo 2020</a>
                    </div>
                </folder>
            </transition>

            <transition name="bounce">
                <folder :data     = "folders.strife"
                    @close    = "folders.strife.visible=false"
                    @minimize = "minimize('strife')"
                    v-show    = "!folders.strife.minimized && folders.strife.visible"
                    v-if      = "folders.strife.visible">
                <div class="strife">
                    <div class="strife-content">
                        <img src="assets/img/strife.png" width="50">
                        <p class="folder-heading">Strife Framework
                            <span class="folder-subheading">A Fast and Lightweight PHP MVC Framework.</span>
                        </p>

                        <img src="assets/img/strife-slides/1.png" v-if="folders.strife.sample==1" class="img-fluid">
                        <img src="assets/img/strife-slides/2.png" v-if="folders.strife.sample==2" class="img-fluid">
                        <img src="assets/img/strife-slides/3.png" v-if="folders.strife.sample==3" class="img-fluid">
                        <img src="assets/img/strife-slides/4.png" v-if="folders.strife.sample==4" class="img-fluid">

                        <div style="display: flex;justify-content: center;">
                            <button class="border" @click="folders.strife.sample=1">Routing</button>
                            <button class="border" @click="folders.strife.sample=2">ORM</button>
                            <button class="border" @click="folders.strife.sample=3">CLI</button>
                            <button class="border" @click="folders.strife.sample=4">Template Engine</button>
                        </div>
                        <br>

                        <p class="description">
                            I am proud to introduce you this powerful PHP Framework I made from scratch.
                            It functions as a full-blown MVC that offers highly understandable coding structure
                            and it tops the benchmarks among the biggest players out in the wild with a small footprint
                            of 500kb a totally standalone package.
                            <br>
                            <br>

                            It has it's own
                            <b>Smart Routing</b>, <b>Query Builder</b>, <b>Object Relational Mapper(ORM)</b>,
                            <b>Migration Manager</b>, <b>Command Line Interface(CLI)</b>, <b>Seeder</b>,
                            <b>Backup/Restore</b>, <b>Template Engine</b>, <b>Custom Tags</b>, <b>HTTP/File Handler</b>
                            <b>Form Builder</b>, <b>Auto-validation</b>, <b>Auto-population</b>,
                            and a bunch of helper functions to boost your development 50x faster!

                            <br>
                            <br>
                            Strife Framework's first release was Nov 2015 and now is on v1.4.2 I am looking
                            for contributors to build on top of it. Please don't hesitate to DM me and check my Github.
                        </p>
                        <ul class="strife-list">
                            <li><a class="git" href="https://github.com/strifejeyz/framework">Github</a></li>
                            <li><a class="site" href="http://strifeframework.com">Official Site</a></li>
                            <li><a class="docs" href="http://docs.strifeframework.cf">Documentation</a></li>
                        </ul>
                    </div>
                </div>
            </folder>
            </transition>

            <transition name="bounce">
                <folder :data     = "folders.projects"
                        @close    = "folders.projects.visible=false"
                        @minimize = "minimize('projects')"
                        v-show    = "!folders.projects.minimized && folders.projects.visible"
                        v-if      = "folders.projects.visible">
                    <div class="folder-heading">
                        <br>
                        <h4 class="">Open-source projects</h4>
                        <span class="folder-subheading">Below are the projects i'm working on.</span>
                    </div>
                    <div class="project-list flex flex-between">
                        <div class="project-item flex flex-left">
                            <ul class="project-details">
                                <a class="title" href="https://github.com/strifejeyz/framework">Strife Framework</a>
                                <span class="status">Update: 2015-present</span>
                                <a class="link" href="http://strifeframework.com">Visit Website</a>
                                <a class="link" href="http://docs.strifeframework.cf">Documentation</a>
                                <a class="link" href="https://github.com/strifejeyz/framework">Github Page</a>
                            </ul>
                            <p class="description">
                                Strife Framework is a PHP MVC framework for small to large scale application development
                                this is my long time running project, it started Nov 2015 and is
                                now at version 1.4.2 stable and it is fully documented.
                                I am looking for contributors to build on top of it
                                and maintain the code. If you are interested, please DM me and fork it.
                            </p>
                        </div>
                    </div>

                    <div class="project-list flex flex-between">
                        <div class="project-item flex flex-left">
                            <ul class="project-details">
                                <a class="title" href="https://github.com/strifejeyz/framework">Strife CMS</a>
                                <span class="status">Update: 2020-present</span>
                                <a class="link" href="https://github.com/strifejeyz/strife-cms">Github Page</a>
                            </ul>
                            <p class="description">
                                Strife is a super lightweight and ultra fast CMS made on top of Strife Framework and Vuejs.
                                It's still under development and it offers a bunch of stuff for managing your website content
                                like Posts,Pages,Menus,Users,Media library, and many more!
                            </p>
                        </div>
                    </div>

                    <div class="project-list flex flex-between">
                        <div class="project-item flex flex-left">
                            <div class="project-details">
                                <a class="title" href="https://github.com/strifejeyz/animator-css">Animator CSS</a>
                                <span class="status">Update: 11/20/2020</span>
                                <a class="link" href="https://github.com/strifejeyz/animator-css">Github Page</a>
                            </div>
                            <p class="description">
                                Animator CSS is a stylesheet dedicated for adding eye-candy effects in your website
                                by simply adding and removing a class name. This Windows95 website is powered by
                                the Animator CSS.
                            </p>
                        </div>
                    </div>

                    <div class="project-list flex flex-between">
                        <div class="project-item flex flex-left">
                            <div class="project-details">
                                <a class="title" href="https://github.com/strifejeyz/windows-95">Windows 95 Experience</a>
                                <span class="status">Update: 04/30/2020</span>
                                <a class="link" href="https://github.com/strifejeyz/windows-95">Github Page</a>
                            </div>
                            <p class="description">
                                Windows 95 Experience is a 1-day project I did for simulating the Microsoft Windows 95 Operating System
                                the full source code can be found <a href="https://github.com/strifejeyz/windows-95">here</a>
                            </p>
                        </div>
                    </div>

                    <div class="project-list flex flex-between">
                        <div class="project-item flex flex-left">
                            <div class="project-details">
                                <a class="title" href="https://github.com/strifejeyz/animator-css">Covid-19 Tracker</a>
                                <span class="status">Update: 03/25/2020</span>
                                <a class="link" href="http://corona-virus.totalh.net">Visit Website</a>
                            </div>
                            <p class="description">
                                Covid-19 tracker was made to provide current stats about the Coronavirus outbreak here
                                in the Philippines and worldwide count. Its last update rolled out on 1st of April 2020.
                            </p>
                        </div>
                    </div>

                </folder>
            </transition>

            <transition name="bounce">
                <folder :data     = "folders.skills"
                        @close    = "folders.skills.visible=false"
                        @minimize = "minimize('skills')"
                        v-show    = "!folders.skills.minimized && folders.skills.visible"
                        v-if      = "folders.skills.visible">

                    <p class="folder-heading">
                        My Technology Stack
                        <span class="folder-subheading">
                        (including but not limited to)
                    </span>
                    </p>

                    <div class="skill-row">
                        <div class="skill-item">
                            <div class="skill-img">
                                <img src="assets/img/skills/php.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/4.png">
                            </div>
                            <div class="skill-name">
                                <div class="skill">PHP(OOP/MVC)</div>
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img src="assets/img/skills/js.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/4.png">
                            </div>
                            <div class="skill-name">
                                Javascript
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img src="assets/img/skills/vue.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/4.png">
                            </div>
                            <div class="skill-name">
                                Vuejs
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/css.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/4.png">
                            </div>
                            <div class="skill-name">
                                CSS/CSS3
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/rest.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/4.png">
                            </div>
                            <div class="skill-name">
                                REST API
                            </div>
                        </div>
                    </div>
                    <div class="skill-row">
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/mysql.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/4.png">
                            </div>
                            <div class="skill-name">
                                MySQL
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/bs.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/4.png">
                            </div>
                            <div class="skill-name">
                                Bootstrap 3/4
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/ionic.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/3.png">
                            </div>
                            <div class="skill-name">
                                IONIC 4
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/ps.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/3.png">
                            </div>
                            <div class="skill-name">
                                Photoshop CS6
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/vegas.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/3.png">
                            </div>
                            <div class="skill-name">
                                Vegas Pro
                            </div>
                        </div>
                    </div>
                    <div class="skill-row">
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/vb.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/3.png">
                            </div>
                            <div class="skill-name">
                                Visual Basic
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/ng.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/3.png">
                            </div>
                            <div class="skill-name">
                                Angular
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/strife.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/5.png">
                            </div>
                            <div class="skill-name">
                                Strife Framework
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/hosting.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/4.png">
                            </div>
                            <div class="skill-name">
                                Web Hosting <br> Administration
                            </div>
                        </div>
                        <div class="skill-item">
                            <div class="skill-img">
                                <img class="skill-img" src="assets/img/skills/seo.png">
                            </div>
                            <div class="skill-star">
                                <img src="assets/img/stars/3.png">
                            </div>
                            <div class="skill-name">
                                SEO
                            </div>
                        </div>
                    </div>
                </folder>
            </transition>

            <transition name="bounce">
                <folder :data = "folders.trash"
                    @close    = "folders.trash.visible=false"
                    @minimize = "minimize('trash')"
                    v-show    = "!folders.trash.minimized && folders.trash.visible"
                    v-if      = "folders.trash.visible">
                    <div style="padding: 100px">
                        <h1>I do <b>shift</b>+<b>delete</b> all the time.</h1>
                    </div>
                </folder>
            </transition>

        </div><!--desktop-->
        <taskbar :folders="folders" @restore="restore" />
    </div>
</template>

<script>
    import dragresize from "./components/dragresize";
    import draggable from "./components/draggable";
    import taskbar from "./components/taskbar";
    import folder from "./components/folder";
    import icon from "./components/icon";

    export default {
        name: "App",
        components: {
            dragresize,
            draggable,
            taskbar,
            folder,
            icon
        },
        data: function () {
            return {
                folders: {
                    intro: {
                        title:   "Welcome Visitor!",
                        id:      "folder-intro",
                        minimized: false,
                        visible: true,
                        items:    1,
                        icon: "assets/img/icons/folder-open.png"
                    },
                    strife: {
                        title:     "Strife Framework!",
                        id:        "folder-strife",
                        minimized: false,
                        visible:   false,
                        sample:    1,
                        items:     4,
                        icon: "assets/img/icons/strife.png"
                    },
                    projects: {
                        title:    "#FOSS",
                        id:        "folder-projects",
                        minimized: false,
                        visible:   false,
                        items:     4,
                        icon: "assets/img/icons/strife.png"
                    },
                    skills: {
                        title:   "My Skills",
                        id:      "folder-skills",
                        minimized: false,
                        visible: false,
                        items:     15,
                        icon: "assets/img/icons/skills.png"
                    },
                    trash: {
                        title:     "Recycle Bin",
                        id:        "folder-trash",
                        minimized: false,
                        visible:   false,
                        items:     0,
                        icon: "assets/img/icons/trash.png"
                    },
                },
                icons: {
                    intro: {
                        name: "Intro",
                        icon: "assets/img/icons/folder-open.png"
                    },
                    projects: {
                        name: "My Projects",
                        icon: "assets/img/icons/computer.png"
                    },
                    strife: {
                        name: "Strife Framework",
                        icon: "assets/img/icons/strife.png"
                    },
                    skills: {
                        name: "My Skills",
                        icon: "assets/img/icons/skills.png"
                    },
                    git:    {
                        name: "Github",
                        icon: "assets/img/icons/git.png"
                    },
                    trash: {
                        name: "Recycle Bin",
                        icon: "assets/img/icons/trash.png"
                    }
                }
            }
        },
        methods: {
            minimize:   function (folderName) {
                if (folderName == "intro") {
                    this.folders.intro.minimized = true;
                }
                if (folderName == "projects") {
                    this.folders.projects.minimized = true;
                }
                if (folderName == "strife") {
                    this.folders.strife.minimized = true;
                }
                if (folderName == "skills") {
                    this.folders.skills.minimized = true;
                }

                if (folderName == "trash") {
                    this.folders.trash.minimized = true;
                }
            },
            restore:    function (folderName) {
                if (folderName == "intro") {
                    this.folders.intro.minimized = false;
                    this.folders.intro.visible = true;
                }
                if (folderName == "projects") {
                    this.folders.projects.minimized = false;
                    this.folders.projects.visible = true;
                }
                if (folderName == "strife") {
                    this.folders.strife.minimized = false;
                    this.folders.strife.visible = true;
                }
                if (folderName == "skills") {
                    this.folders.skills.minimized = false;
                    this.folders.skills.visible = true;
                }

                if (folderName == "trash") {
                    this.folders.trash.minimized = false;
                    this.folders.trash.visible = true;
                }
            },
            goToGithub: function () {
                window.open(
                    'https://github.com/strifejeyz/windows-95',
                    '_blank'
                );
            },
            fullscreen: function () {
                let app = document.querySelector("html");
                app.requestFullscreen();
            }
        },
    };
</script>

<style scoped>

    .bounce-enter-active {
        animation: bounce-in .5s;
    }
    .bounce-leave-active {
        animation: bounce-in .3s reverse;
    }

    @keyframes bounce-in {
        0% {
            transform: scale(0);
        }
        50% {
            transform: scale(1.3);
        }
        100% {
            transform: scale(1);
        }
    }

</style>