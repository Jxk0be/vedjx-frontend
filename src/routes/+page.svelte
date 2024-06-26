<script>
    import HeroImage from "$lib/data/TN_theatre.jpg"
    import Logo from '$lib/data/LogoWhite.svg?raw';
</script>

<div class="relative w-full max-w-[1500px] mx-auto h-screen flex items-center justify-center">
    <div class="absolute inset-0 bg-cover bg-center flex justify-center items-center"
         style={`background-image: url(${HeroImage});`}>
        <div class="absolute inset-0 bg-slate-900 bg-opacity-90"></div>
        <div class="relative z-10 text-center text-white">
            <div class="font-bold min-w-[400px] py-[10px]">
                {@html Logo}
            </div>
            <p class="text-lg md:text-xl font-bold">Based in Knoxville, TN</p>
        </div>
    </div>
</div>