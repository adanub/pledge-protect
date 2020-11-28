<template>
    <div class="post-container post-inactive" id="pledge-post">
        <div class="blurred-background crossfade1" @click="ClosePledgePost()"></div>
        <div class="content-box shrink">
            <div class="top-bar">Pledge<button @click="ClosePledgePost()"><close-icon class="close-button"/></button></div>
            <app-logo class="applogo" :width="113.75" :height="122.55"/>
            <div class="writing text-grey text-4xl text-left sm:text-center">I, Harrison, <br> from Inner City, <br> pledge to...</div>
            <div class="pt-8 writing pledge-pink text-3xl text-left sm:text-center">Physical distance. <br> Practice good hygeine.</div>
            <div class=" pt-8 writing text-grey text-4xl text-left sm:text-center">I'm pledging for...</div>
            <div class="pt-8 writing pledge-pink text-3xl text-left sm:text-center">The good of the community.</div>
        </div>
    </div>
</template>

<script>
import CloseIcon from '~/components/Icons/CloseIcon.vue'
import AppLogo from '~/components/Icons/Logo.vue'

export default {
    components: {
        CloseIcon,
        AppLogo
    },
    methods: {
        ClosePledgePost() {
            var post = document.getElementById('pledge-post');
            if (post != null) {
                post.classList.add('post-inactive');
            }
        }
    }
}
</script>

<style scoped>
    .post-container {
        margin: 0 auto;
        min-height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        transition: 1s ease-out;
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
        font-family: 'Kalam', cursive;
        padding-left: 10%;
        padding-right: 10%;
    }

    .applogo {
        margin: 60px auto;
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
        background-color: #F34A63;
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
        position: absolute;
        left: 0;
        top: 0;
        
        backdrop-filter: blur(4px);
        -webkit-backdrop-filter: blur(4px);

        background-color: rgba(0, 0, 0, 0.2);
    }

    .content-box {
        display: block;
        position: absolute;
        background-color: #f5f5f5;
        width: 90vw;
        height: 90vh;
        min-width: 300px;
        max-width: 800px;
        z-index: 5;
        border-radius: 16px;
        box-shadow: 0px 4px 16px rgba(0, 0, 0, 0.25);
        top: 16px;
        padding-top: 18px;
        padding-bottom: 18px;
        overflow: hidden;
    }
</style>