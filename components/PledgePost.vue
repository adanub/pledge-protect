<template>
    <div class="post-container post-inactive" id="pledge-post">
        <div class="blurred-background crossfade1" @click="ClosePledgePost()"></div>
        <div class="content-box shrink">
            <div class="top-bar">
                <svg class="top-logo" width="30" height="30" viewBox="0 0 30 30" fill="none" xmlns="http://www.w3.org/2000/svg"> <path fill-rule="evenodd" clip-rule="evenodd" d="M3.91731 26.25C3.74595 25.8689 3.69834 25.4292 3.81134 24.9936L5.92759 16.8336C6.16226 15.9285 7.01183 15.3446 7.91286 15.4133H16.9102C17.7408 15.4133 18.5041 15.6356 19.1038 16.0658C19.2138 16.1097 19.3218 16.1636 19.4264 16.2274L22.6503 18.192L22.2237 12.2747C22.1538 11.3031 22.8857 10.4575 23.8573 10.3875C24.8289 10.3174 25.6745 11.0494 25.7445 12.021L26.4339 21.5857C26.4668 22.0415 26.3231 22.4695 26.061 22.8029C25.4405 23.8141 24.1162 24.1337 23.1023 23.5159L20.4029 21.8708V26.25H9.40937L16.0041 22.1245C17.0126 21.4935 17.3193 20.1624 16.6883 19.1539C16.0574 18.1454 14.7263 17.8387 13.7178 18.4698L8.60599 21.6675L8.43704 22.3191L13.9788 18.935C14.7322 18.475 15.7174 18.7131 16.1774 19.4666C16.6376 20.2199 16.3994 21.2052 15.6459 21.6652L8.13776 26.25H3.91731ZM13.4958 3.75C16.4911 3.75 18.923 6.18188 18.923 9.17723C18.923 12.1726 16.4911 14.6045 13.4958 14.6045C10.5004 14.6045 8.06853 12.1726 8.06853 9.17723C8.06853 6.18188 10.5004 3.75 13.4958 3.75Z" fill="white"/> </svg> 
                Pledge
                <button @click="ClosePledgePost()"><close-icon class="close-button"/></button>
            </div>
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
    .top-logo {
        position: absolute;
        top: 3px;
        left: 7px;
        width: 30px;
        height: 30px;
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
        margin-left: auto;
    }
</style>