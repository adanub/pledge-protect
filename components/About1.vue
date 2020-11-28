<template>
    <div class="post-container" id="protect-post">
        <div class="blurred-background crossfade1" @click="CloseProtectPost()"></div>
        <div class="content-box crossfade1">
            <div class="top-bar">Protect<button @click="CloseProtectPost()"><close-icon class="close-button"/></button></div>

            <div class="writing header1 protect-green">Jared</div>
            <div class="writing location">from Western Sydney</div>
            <div class="image-holder"><img src="img/jared_profile.png"></div>
            <div class="writing header2 protect-green">Who am I?</div>
            <div class="writing paragraph text-grey">This is my dad, Jared. He loves his veggie garden, and tends to it every morning, rain or shine. He's the most caring person I know, and I really want to help him stay safe during this pandemic!</div>
            <div class="writing header2 protect-green">Why am I here?</div>
            <div class="writing paragraph text-grey">According to official information from NSW Health, Jared is vulnerable to COVID-19 due to his age and being diabetic, and he will likely fall seriously ill if he catches it.</div>
        </div>
    </div>
</template>

<script>
import CloseIcon from '~/components/Icons/CloseIcon.vue'

export default {
    components: {
        CloseIcon
    },
    methods: {
        NextPage() {
            var page = document.getElementById('about1');
            if (page != null) {
                page.classList.add('page-inactive');
            }
        }
    }
}
</script>

<style scoped>
    .image-holder {
        width: 82%;
        max-width: 500px;
        padding-bottom: 0;
        max-height: 200px;
        border-radius: 11px;
        overflow: hidden;
        background-color: #0FA78E;
        margin: 10px auto;
    }
    .image-holder img {
        width: 100%;
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
        transition: 1s ease-out;
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
        font-family: 'Heebo', sans-serif;
    }
</style>