<script>
    import KnoxvilleVid from "$lib/data/knoxville.mp4"
    import Logo from '$lib/data/LogoWhite.svg?raw';
</script>

<div class="w-full h-screen text-white relative custom-gradient">
    <video src={KnoxvilleVid} class="w-full z-[5] h-full object-cover" muted autoplay loop playsinline controls webkit-playsinline />
    <div class="absolute w-full z-[12] h-full top-0 left-0 bg-black/70" />
    <div class="absolute top-0 w-full h-full flex flex-col justify-center items-center z-[15]">
        <div class="font-bold md:min-w-[400px] min-w-[300px] py-[10px]">
            {@html Logo}
        </div>
        <p class="text-lg md:text-xl font-bold">Based in Knoxville, TN</p>
    </div>
</div>