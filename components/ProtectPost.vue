<template>
    <div class="post-container post-inactive" id="protect-post">
        <div class="blurred-background crossfade1" @click="CloseProtectPost()"></div>
        <div class="content-box shrink">
            <div class="top-bar">
                <svg class="top-logo" width="30" height="30" viewBox="0 0 30 30" fill="none" xmlns="http://www.w3.org/2000/svg"> <path fill-rule="evenodd" clip-rule="evenodd" d="M15.0061 3.75401L26.2503 7.66816C26.2503 7.66816 25.8824 15.5119 24.1843 18.8926C21.0843 25.0643 15.2991 26.278 15.0061 26.3359V26.3381L15.0003 26.337L14.9946 26.3381V26.3359C14.7015 26.278 8.91636 25.0643 5.81631 18.8926C4.11818 15.5119 3.75031 7.66816 3.75031 7.66816L14.9946 3.75401V3.75L15.0003 3.752L15.0061 3.75V3.75401V3.75401ZM21.2224 20.6905H8.77826C8.77826 17.2564 11.5663 14.4684 15.0003 14.4684C18.4344 14.4684 21.2224 17.2564 21.2224 20.6905V20.6905ZM15.0003 7.65972C16.6243 7.65972 17.9427 8.97813 17.9427 10.602C17.9427 12.2259 16.6243 13.5444 15.0003 13.5444C13.3764 13.5444 12.058 12.2259 12.058 10.602C12.058 8.97813 13.3764 7.65972 15.0003 7.65972Z" fill="white"/> </svg> 
                Protect
                <button @click="CloseProtectPost()"><close-icon class="close-button"/></button>
            </div>
            <div class="writing header1 protect-green">Jared</div>
            <div class="writing location">from Western Sydney</div>
            <div class="image-holder"><img src="img/jared_profile.png"></div>
            <div id="protectpost1">
                <div class="writing header2 protect-green">Who am I?</div>
                <div class="writing paragraph text-grey">This is my dad, Jared. He loves his veggie garden, and tends to it every morning, rain or shine. He's the most caring person I know, and I really want to help him stay safe during this pandemic!</div>
                <div class="writing header2 protect-green">Why am I here?</div>
                <div class="writing paragraph text-grey">According to official information from NSW Health, Jared is vulnerable to COVID-19 due to his age and having chronic medical conditions, and he will likely fall seriously ill if he catches it.</div>
                <div class="protect-button-base float-right"><button class="protect-button" onclick="this.blur();" @click="NextPage()"><div class="inline-block">Help keep Jared safe</div></button></div>
            </div>
            <div id="protectpost2" class="inactive">
                <div class="writing header2 protect-green">To help keep Jared safe,</div>
                <button class="dropdown" on-click="this.blur();">
                    <svg class="absolute" width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"> <path d="M20 6L9 17L4 12" stroke="#0FA78E" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/> </svg> 
                    <p class="dropdown-title">Practice physical distancing</p>
                    <dropdown class="dropdown-icon"/>
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import CloseIcon from '~/components/Icons/CloseIcon.vue'
import Dropdown from '~/components/Icons/Dropdown.vue'

export default {
    components: {
        CloseIcon,
        Dropdown
    },
    methods: {
        CloseProtectPost() {
            var post = document.getElementById('protect-post');
            if (post != null) {
                post.classList.add('post-inactive');
                this.PrevPage();
            } else {
                console.log('Protect post could not be found in the DOM');
            }
        },

        NextPage() {
            var page1 = document.getElementById('protectpost1');
            var page2 = document.getElementById('protectpost2');

            if (page1 != null && page2 != null) {
                page1.classList.add('inactive');
                page2.classList.remove('inactive');
            } else {
                console.log("Protect post pages could not be found in the DOM");
            }
        },

        PrevPage() {
            var page1 = document.getElementById('protectpost1');
            var page2 = document.getElementById('protectpost2');

            if (page1 != null && page2 != null) {
                page1.classList.remove('inactive');
                page2.classList.add('inactive');
            } else {
                console.log("Protect post pages could not be found in the DOM");
            }
        }
    }
}
</script>

<style scoped>
    .dropdown-title {
        position: absolute;
        margin-left: 40px;
    }

    .dropdown-icon {
        float: right;
        right: 8px;
    }

    .dropdown {
        padding: 9px;
        background-color: #F6F6F6;
        border-radius: 12px;
        width: 90%;
        height: 39px;
        margin: 10px 5%;
        box-shadow: 0px 2px 6px rgba(0, 0, 0, 0.25);
        outline: none;
        font-family: 'Kalam', cursive;
        font-weight: 700;
        font-size: 16px;
        overflow: hidden;
    }

    #protectpost1 {
        position: absolute;
        width: 100%;
    }

    #protectpost2 {
        position: absolute;
        width: 100%;
    }

    .inactive {
        visibility: hidden;
        opacity: 0;
        transition: 0.2s ease-out;
    }

    .protect-button-base {
        position: fixed;
        background-color: #0C8A75;
        border-radius: 8px;

        margin: 16px 12%;
        height: 52px;
        min-width: 261px;
        width: 76%;

        bottom: 20px;
    }
    .protect-button {
        background-color: #0FA78E;
        border-radius: 8px;

        color: white;
        font-family: 'Kalam', cursive;
        font-weight: 700;
        letter-spacing: 0.03rem;
        font-size: 18px;
        padding-top: 6px;

        margin: -4px auto;
        height: 52px;
        min-width: 261px;
        width: 100%;

        transition: 0.1s;
    }
    .protect-button:hover, .protect-button:focus {
        outline: none;
        transform: translateY(-4px);

        transition: 0.1s;
    }
    .protect-button:active {
        transform: translateY(3px);
    }

    .top-logo {
        position: absolute;
        top: 4px;
        left: 7px;
        width: 30px;
        height: 30px;
    }

    .image-holder {
        width: 82%;
        max-width: 500px;
        padding-bottom: 0;
        max-height: 200px;
        border-radius: 11px;
        overflow: hidden;
        margin: 10px auto;
        box-shadow: inset 0px 0px 10px rgba(0, 0, 0, 0.25);
    }
    .image-holder img {
        position: relative;
        width: 100%;
        z-index: -2;
    }

    .header1 {
        font-size: 32px;
        line-height: 34px;
        text-align: left;
        font-weight: 700;

        margin-top: 40px;
    }

    .location {
        font-family: 'Kalam', cursive;
        font-size: 16px;
        line-height: 18px;
        font-weight: 400;
        text-align: left;
        color: #3B413C;

        margin-top: 10px;
    }

    .header2 {
        font-size: 18px;
        line-height: 18px;
        text-align: left;
        font-weight: 700;

        margin-top: 30px;
    }

    .paragraph {
        font-size: 16px;
        line-height: 24px;
        text-align: left;
        font-weight: 400;

        margin-top: 15px;
    }

    .post-container {
        margin: 0 auto;
        min-height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        transition: 0.5s ease-out;
        width: 100vw;
    }

    .post-container.post-inactive {
        visibility: hidden;
        transition: 0.5s ease-in-out;
    }

    .crossfade1 {
        visibility: visible;
        opacity: 1;
        transition: 0.3s ease-out;
    }
    .post-inactive .crossfade1 {
        visibility: hidden;
        opacity: 0;
        transition: 0.5s ease-in-out;
        overflow: hidden;
    }

    .shrink {
        visibility: visible;
        transform: scale(1);
        opacity: 1;
        transition: 0.3s ease-out;
    }
    .post-inactive .shrink {
        visibility: hidden;
        transform: scale(0.5);
        opacity: 0;
        transition: 0.5s ease-in-out;
    }

    @keyframes fade-in {
        0% {
            opacity: 0;
        }
        100% {
            opacity: 1;
        }
    }

    .writing {
        padding-left: 10%;
        padding-right: 10%;
    }

    .close-button {
        position: absolute;
        top: -5px;
        right: -3px;
        width: 48px;
        height: 48px;
    }
    .close-button:hover {
        transform: scale(1.3);
        transition: all 0.4s cubic-bezier(0.47, 2.64, .41, .8);
    }

    .top-bar {
        width: 100%;
        background-color: #0FA78E;
        height: 38px;

        position: absolute;
        top: 0;

        border-radius: 16px 16px 0px 0px;
        box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.25);

        font-family: 'Kalam', cursive;
        font-weight: 700;
        font-size: 1.4rem;
        text-align: center;
        color: #FFFFFF;
        vertical-align: middle;

        padding-top: 5px;
    }

    .blurred-background {
        width: 100vw;
        height: 100vh;
        position: fixed;
        left: 0;
        top: 0;
        
        backdrop-filter: blur(4px);
        -webkit-backdrop-filter: blur(4px);

        background-color: rgba(0, 0, 0, 0.2);
    }

    .content-box {
        display: block;
        position: fixed;
        background-color: #f5f5f5;
        width: 90vw;
        height: 96vh;
        min-width: 300px;
        max-width: 800px;
        z-index: 5;
        border-radius: 16px;
        box-shadow: 0px 4px 16px rgba(0, 0, 0, 0.25);
        top: 16px;
        padding-top: 18px;
        padding-bottom: 18px;
        overflow: hidden;
        font-family: 'Heebo', sans-serif;
    }
</style>