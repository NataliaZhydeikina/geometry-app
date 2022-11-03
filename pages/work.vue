<template>
  <div class="bg-black h-screen px-6 py-6">
    <h2
      class="text-white uppercase font-exo text-5xl font-bold opacity-0"
      ref="title"
      style="transform: translateY(30px)"
    >
      My Work
    </h2>
    <div class="flex space-x-4">
      <div
        ref="projects"
        v-for="project in projects"
        class="w-full opacity-0"
        style="transform: translateX(30px)"
      >
        <nuxt-link to="/">
          <img
            :src="project.image.url"
            alt="notebook"
            style="height: 70vh"
            class="object-cover"
          />
          <p class="text-white font-space-mono text-sm uppercase">
            {{ project.title }}
          </p>
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<style>
.font-exo {
  font-family: "Exo 2", sans-serif;
  font-style: italic;
  text-transform: uppercase;
}
.font-space-mono {
  font-family: "Space Mono", monospace;
}
</style>

<script>
import gsap from "gsap";
import notebookImg from "~/assets/imgs/notebook.jpg";
import notebooksImg from "~/assets/imgs/notebooks.jpg";
import programmerImg from "~/assets/imgs/programmer.jpg";
import workPlaceImg from "~/assets/imgs/work place.jpg";
import workspaceImg from "~/assets/imgs/workspace.jpg";
import keyboard from "~/assets/imgs/keyboard.jpg";

export default {
  data() {
    return {
      projects: [
        {
          image: {
            url: notebookImg,
          },
          title: "notebook",
        },
        {
          image: {
            url: notebooksImg,
          },
          title: "notebooks",
        },
        {
          image: {
            url: programmerImg,
          },
          title: "programmer",
        },
        {
          image: {
            url: workPlaceImg,
          },
          title: "work place",
        },
        {
          image: {
            url: workspaceImg,
          },
          title: "workspace",
        },
        {
          image: {
            url: keyboard,
          },
          title: "keyboard",
        },
      ],
    };
  },
  mounted() {
    gsap.to(this.$refs.title, {
      opacity: 1,
      duration: 1.5,
      y: 0,
      ease: "expo",
    });
    gsap.to(this.$refs.projects, {
      opacity: 1,
      duration: 1.5,
      stagger: 0.1,
      x: 0,
      ease: "expo",
      delay: 0.2,
    });
  },
};
</script>
