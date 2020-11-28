<template>
    <client-only><button onclick="this.blur();" @click="OpenPledgePost();" class="pin-marker">
        <svg width="65" height="86" viewBox="0 0 65 86" fill="none" xmlns="http://www.w3.org/2000/svg">
            <g filter="url(#filter0_d)">
            <path d="M32.0416 52.0577C45.3123 52.0577 56.0704 41.2996 56.0704 28.0289C56.0704 14.7581 45.3123 4 32.0416 4C18.7708 4 8.0127 14.7581 8.0127 28.0289C8.0127 41.2996 18.7708 52.0577 32.0416 52.0577Z" fill="#F34A63"/>
            <path fill-rule="evenodd" clip-rule="evenodd" d="M32.0417 73.9916C32.0417 73.9916 7.35805 43.9873 8.0128 28.0294C8.26887 21.7873 56.1584 22.4045 56.0705 27.6867C55.7432 47.3719 32.0417 73.9916 32.0417 73.9916Z" fill="#F34A63"/>
            <g clip-path="url(#clip0)">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M20.1842 40.8169C19.9955 40.3638 19.9431 39.841 20.0675 39.3232L22.3972 29.6214C22.6556 28.5453 23.5908 27.8512 24.5827 27.9328H34.4876C35.4019 27.9328 36.2423 28.1971 36.9024 28.7085C37.0236 28.7608 37.1424 28.8248 37.2576 28.9006L40.8067 31.2365L40.3371 24.2012C40.26 23.0461 41.0658 22.0407 42.1354 21.9575C43.205 21.8741 44.1359 22.7444 44.2129 23.8995L44.9719 35.2713C45.0081 35.8133 44.8499 36.3221 44.5614 36.7185C43.8783 37.9207 42.4204 38.3008 41.3042 37.5662L38.3326 35.6103V40.8169H26.2302L33.4901 35.9119C34.6004 35.1618 34.9379 33.5792 34.2434 32.3801C33.5488 31.181 32.0835 30.8165 30.9732 31.5667L25.3458 35.3686L25.1598 36.1433L31.2605 32.1199C32.0899 31.573 33.1745 31.856 33.6809 32.7518C34.1874 33.6475 33.9253 34.819 33.0958 35.3659L24.8303 40.8169H20.1842ZM30.7288 14.0659C34.0262 14.0659 36.7034 16.9572 36.7034 20.5185C36.7034 24.0798 34.0262 26.9711 30.7288 26.9711C27.4313 26.9711 24.7541 24.0798 24.7541 20.5185C24.7541 16.9572 27.4313 14.0659 30.7288 14.0659Z" fill="white"/>
            </g>
            </g>
            <defs>
            <filter id="filter0_d" x="0" y="0" width="64.0707" height="85.9916" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
            <feFlood flood-opacity="0" result="BackgroundImageFix"/>
            <feColorMatrix in="SourceAlpha" type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0"/>
            <feOffset dy="4"/>
            <feGaussianBlur stdDeviation="4"/>
            <feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.25 0"/>
            <feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow"/>
            <feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow" result="shape"/>
            </filter>
            <clipPath id="clip0">
            <rect width="25" height="27" fill="white" transform="translate(20 14)"/>
            </clipPath>
            </defs>
        </svg>
    </button></client-only>
</template>

<script>
export default {
    methods: {
        OpenPledgePost() {
            var post = document.getElementById('pledge-post');
            if (post != null) {
                post.classList.remove('post-inactive');
            }
        }
    }
}
</script>