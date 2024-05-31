<script>
  let isHovered1 = false;
  let isHovered2 = false;
  let isHovered3 = false;
  let isHovered4 = false;
  let isHovered5 = false;
  let isHovered6 = false;
</script>

<section class="m-auto max-w-7xl flex flex-col my-16 gap-3 px-5">
  <div class="flex justify-end items-center gap-4 rounded-lG">
    <i class="fa-solid fa-magnifying-glass"></i><input
      class="p-2 rounded-lg"
      type="text"
      placeholder="Palabra Clave"
    />
  </div>

  <hr class="border-p border-gray-300" />

  <div class="flex flex-col gap-2">
    <h1 class="font-bold text-center text-[20px]">Ultimos Podcasts</h1>
    <div class="gap-4 grid grid-cols-3 grid-rows-2">
      <div class="relative bg-[#E6E6E6] gap-2 flex flex-col cursor-pointer shadow rounded-lg p-2 transition-all hover:scale-105"
        on:mouseenter={() => (isHovered1 = true)}
        on:mouseleave={() => (isHovered1 = false)}>
        {#if isHovered1}
          <button
            class="absolute bottom-[90px] right-[20px] flex items-center justify-center">
            <img src="src/lib/images/play.webp" alt="" width="40px" />
          </button>
        {/if}
        <img class="bg-white p-2 rounded-lg g" src="src/lib/images/cditi-radio-logo.webp"
          alt=""/>
        <div>
          <h1 class="font-bold text-[20px]">podcast</h1>
          <label for="">Lorem ipsum dolor sit amet consectetur </label>
        </div>
      </div>

      <div class="relative bg-[#E6E6E6] gap-2 flex flex-col cursor-pointer shadow rounded-lg p-2 transition-all hover:scale-105"
        on:mouseenter={() => (isHovered2 = true)}
        on:mouseleave={() => (isHovered2 = false)} >
        {#if isHovered2}
          <button
          class="absolute bottom-[90px] right-[20px] flex items-center justify-center">
            <img src="src/lib/images/play.webp" alt="" width="40px" />
          </button>
        {/if}
        <img class="bg-white p-2 rounded-lg g" src="src/lib/images/cditi-radio-logo.webp"
          alt=""/>
        <div>
          <h1 class="font-bold text-[20px]">podcast</h1>
          <label for="">Lorem ipsum dolor sit amet consectetur </label>
        </div>
      </div>


      <div class="relative bg-[#E6E6E6] gap-2 flex flex-col cursor-pointer shadow rounded-lg p-2 transition-all hover:scale-105"
        on:mouseenter={() => (isHovered3 = true)}
        on:mouseleave={() => (isHovered3 = false)} >
        {#if isHovered3}
          <button
          class="absolute bottom-[90px] right-[20px] flex items-center justify-center">
            <img src="src/lib/images/play.webp" alt="" width="40px" />
          </button>
        {/if}
        <img class="bg-white p-2 rounded-lg g" src="src/lib/images/cditi-radio-logo.webp"
          alt=""/>
        <div>
          <h1 class="font-bold text-[20px]">podcast</h1>
          <label for="">Lorem ipsum dolor sit amet consectetur </label>
        </div>
      </div>


      <div class="relative bg-[#E6E6E6] gap-2 flex flex-col cursor-pointer shadow rounded-lg p-2 transition-all hover:scale-105"
        on:mouseenter={() => (isHovered4 = true)}
        on:mouseleave={() => (isHovered4 = false)}>
        {#if isHovered4}
          <button
            class="absolute bottom-[90px] right-[20px] flex items-center justify-center">
            <img src="src/lib/images/play.webp" alt="" width="40px" />
          </button>
        {/if}
        <img class="bg-white p-2 rounded-lg g" src="src/lib/images/cditi-radio-logo.webp"
          alt=""/>
        <div>
          <h1 class="font-bold text-[20px]">podcast</h1>
          <label for="">Lorem ipsum dolor sit amet consectetur </label>
        </div>
      </div>

      <div class="relative bg-[#E6E6E6] gap-2 flex flex-col cursor-pointer shadow rounded-lg p-2 transition-all hover:scale-105"
        on:mouseenter={() => (isHovered5 = true)}
        on:mouseleave={() => (isHovered5 = false)} >
        {#if isHovered5}
          <button
          class="absolute bottom-[90px] right-[20px] flex items-center justify-center">
            <img src="src/lib/images/play.webp" alt="" width="40px" />
          </button>
        {/if}
        <img class="bg-white p-2 rounded-lg g" src="src/lib/images/cditi-radio-logo.webp"
          alt=""/>
        <div>
          <h1 class="font-bold text-[20px]">podcast</h1>
          <label for="">Lorem ipsum dolor sit amet consectetur </label>
        </div>
      </div>


      <div class="relative bg-[#E6E6E6] gap-2 flex flex-col cursor-pointer shadow rounded-lg p-2 transition-all hover:scale-105"
        on:mouseenter={() => (isHovered6 = true)}
        on:mouseleave={() => (isHovered6 = false)} >
        {#if isHovered6}
          <button
          class="absolute bottom-[90px] right-[20px] flex items-center justify-center">
            <img src="src/lib/images/play.webp" alt="" width="40px" />
          </button>
        {/if}
        <img class="bg-white p-2 rounded-lg g" src="src/lib/images/cditi-radio-logo.webp"
          alt=""/>
        <div>
          <h1 class="font-bold text-[20px]">podcast</h1>
          <label for="">Lorem ipsum dolor sit amet consectetur </label>
        </div>
      </div>

      
    </div>
  </div>
</section>
