<!--HTML-->


<template>
    <div
            class="app-container app-theme-white body-tabs-shadow fixed-sidebar fixed-header"
            :class="[is_active ? 'closed-sidebar' : null]"
    >
        <Navbar @collapse-sidebar="collapseSideBar"/>

        <div class="app-main">
            <Sidebar/>

            <div class="app-main__outer">
                <slot name="content"></slot>
            </div>
            
        </div>
        
    </div>
    <Footer/>
</template>


<script>
    import Sidebar from "./partials/Sidebar";
    import Navbar from "./partials/Navbar";
    import Footer from "./partials/Footer";
    import "../../assets/scripts/main";
    // import ".../scripts/main.js";
    // import '@/';

    // function asyncSetTimeout(timeout) {
    //   return new Promise((resolve) => setTimeout(resolve, timeout));
    // }

    export default {
        data() {
            return {
                is_active: false,
            };
        },
        name: "Master",
        components: {Navbar, Sidebar,Footer},
        methods: {
            collapseSideBar(is_active) {
                console.log(is_active);
                this.is_active = is_active;
            },
        },
        // props: {
        //   timeout: {
        //     type: Number,
        //     required: true,
        //   },
        //   shouldFail: {
        //     type: Boolean,
        //     default: false,
        //   },
        // },
        // async setup(props) {
        //   await asyncSetTimeout(props.timeout);

        //   if (props.shouldFail) {
        //     throw new Error("Some error happened");
        //   }
        // },
    };
</script>


<style>
    @import "../../main.css";
</style>

