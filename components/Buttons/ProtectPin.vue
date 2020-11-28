<template>
     <client-only><button onclick="this.blur();" @click="OpenProtectPost();" class="pin-marker">
        <svg width="65" height="86" fill="none" xmlns="http://www.w3.org/2000/svg"><g filter="url(#filter0_d)"><path d="M32.042 52.058c13.27 0 24.028-10.758 24.028-24.03C56.07 14.759 45.312 4 32.042 4 18.77 4 8.012 14.758 8.012 28.029c0 13.27 10.759 24.029 24.03 24.029z" fill="#0FA78E"/><path fill-rule="evenodd" clip-rule="evenodd" d="M32.042 73.992S7.358 43.987 8.012 28.029c.257-6.242 48.146-5.625 48.059-.342-.328 19.685-24.03 46.305-24.03 46.305z" fill="#0FA78E"/><g clip-path="url(#clip0)"><path fill-rule="evenodd" clip-rule="evenodd" d="M32.475 17.005l14.404 5.014s-.472 10.048-2.647 14.379c-3.971 7.906-11.382 9.46-11.757 9.535v.003l-.008-.002-.007.002v-.003c-.376-.074-7.787-1.63-11.758-9.535-2.175-4.33-2.646-14.379-2.646-14.379l14.404-5.014V17l.007.003.008-.003v.005zm7.963 21.696H24.497a7.974 7.974 0 017.97-7.97c4.4 0 7.97 3.571 7.97 7.97zm-7.97-16.693a3.77 3.77 0 013.768 3.77c0 2.08-1.689 3.769-3.769 3.769a3.77 3.77 0 01-3.769-3.77 3.77 3.77 0 013.77-3.769z" fill="#fff"/></g></g><defs><clipPath id="clip0"><path fill="#fff" transform="translate(18 17)" d="M0 0h29v29H0z"/></clipPath><filter id="filter0_d" x="0" y="0" width="64.071" height="85.992" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB"><feFlood flood-opacity="0" result="BackgroundImageFix"/><feColorMatrix in="SourceAlpha" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0"/><feOffset dy="4"/><feGaussianBlur stdDeviation="4"/><feColorMatrix values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.25 0"/><feBlend in2="BackgroundImageFix" result="effect1_dropShadow"/><feBlend in="SourceGraphic" in2="effect1_dropShadow" result="shape"/></filter></defs></svg>
    </button></client-only>
</template>

<script>
export default {
    methods: {
        OpenProtectPost() {
            var post = document.getElementById('protect-post');
            if (post != null) {
                post.classList.remove('post-inactive');
            }
        }
    }
}
</script>