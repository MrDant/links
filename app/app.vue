<template>
  <div id="container">
    <div
      class="bubles"
      v-for="b in bubles"
      :key="b"
      :style="{
        width: b.size,
        height: b.size,
        top: b.position.y + 'px',
        left: b.position.x + 'px',
      }"
    ></div>
    <div class="filter"></div>
    <div id="screen">
      <div
        class="bg-slate-400 absolute top-0 left-1/2 -translate-x-1/2 w-28 h-2 rounded-b-md"
      ></div>
      <div class="self-center rounded-full overflow-hidden w-24 h-24 my-4">
        <NuxtPicture src="/moi.jpeg" class="w-full h-full" />
      </div>
      <div class="text-center">
        <h1 class="bold">Maël D'ANTUONO</h1>
        <p class="text-sm italic">
          Développeur fullstack expérimenté en ReactJS, Vue, et Nuxt, passionné
          par les applications web performantes et intuitives.
        </p>
      </div>
      <div>
        <MyLink v-for="link in defaultLink" :key="link.href" :link="link" />
      </div>
      <div>
        <h2 class="underline">Mes projets :</h2>
        <MyLink v-for="link in projectLink" :key="link.href" :link="link" />
      </div>
      <p class="text-xs italic text-end">by Diverty Dev</p>
    </div>
  </div>
</template>
<script setup>
const defaultLink = [
  {
    icon: "i-grommet-icons-chrome",
    label: "Diverty Dev",
    href: "https://divertydev.com",
  },
  {
    icon: "i-grommet-icons-github",
    label: "GitHub",
    href: "https://github.com/MrDant",
  },
];
const projectLink = [
  {
    icon: "i-grommet-icons-chrome",
    label: "Fallo",
    href: "https://fallo.divertydev.com",
  },
];
const bubles = ref([]);
function random(min, max) {
  return Math.floor(Math.random() * (max - min + 1) + min);
}

const interval = ref(null);
onMounted(() => {
  const nb = random(30, 200);
  bubles.value = [...Array(nb)].map(() => {
    const size = random(5, 20) + "px";
    const position = {
      x: random(0, window.screen.width),
      y: random(0, window.screen.height),
    };
    const speed = random(0, 3);
    const direction = { x: random(-1, 1), y: random(-1, 1) };
    return { size, position, speed, direction };
  });
  interval.value = setInterval(() => {
    bubles.value = bubles.value.map((e) => {
      e.position.x += e.speed * e.direction.x;
      e.position.y += e.speed * e.direction.y;
      if (e.position.x > window.screen.width || e.position.x <= 0) {
        e.direction.x *= -1;
      }
      if (e.position.y > window.screen.height || e.position.y <= 0) {
        e.direction.y *= -1;
      }
      return e;
    });
  }, 50);
});
onBeforeUnmount(() => {
  clearInterval(interval.value);
});
</script>

<style lang="scss" scoped>
#container {
  @apply flex items-center justify-center w-screen h-screen text-white bg-black bg-opacity-90;
}
#screen {
  @apply rounded-3xl border-4 border-slate-400 shadow max-w-md w-1/4 min-w-80 p-4 flex flex-col gap-4;
  @apply relative;
  background: rgb(0, 95, 143);
  background: linear-gradient(
    135deg,
    rgba(0, 95, 143, 1) 0%,
    rgba(0, 5, 27, 1) 100%
  );
}
a {
  @apply border border-white w-full hover:bg-slate-100 hover:text-slate-800 cursor-pointer block;
  @apply rounded-full text-center p-2 my-2;
}
.filter {
  @apply absolute top-0 left-0 right-0 bottom-0;
  background-image: url("/bg.jpg");
  background-size: cover;
  background-position-y: bottom;
  mix-blend-mode: multiply;
}
.bubles {
  @apply absolute top-0 left-0 rounded-full bg-white;
}
</style>
