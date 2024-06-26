<template>
  <section ref="techStackSectionEl" class="flex flex-col items-center gap-5">
    <div v-show="isSectionVisible" class="flex flex-col items-center gap-5">
      <h2 class="text-2xl border-b-2 border-primary pb-2" :class="{'animate-fade-in-right' : isSectionVisible}">Tech Stack</h2>
      <div class="flex flex-wrap px-20 lg:p-0 md:flex-row gap-5 md:gap-10 mt-4 items-center justify-center">

        <div v-for="(stack, index) of techStacksDisplayed"
             :class="{'animate-fade-in-left' : isSectionVisible && index % 2 === 0, 'animate-fade-in-right' : isSectionVisible && index % 2 !== 0}">
          <div v-if="stack.type === 'multiple'" class="flex flex-row gap-10 md:gap-5 items-center justify-center p-4 rounded hover:shadow-custom-shadow-primary duration-150 hover:-translate-y-1.5">
            <div v-for="tech of stack.techs" class="flex flex-col gap-2 items-center w-14">
              <NuxtImg :src="tech.icon" :alt="tech.label" class="w-full" :title="tech.label" />
              <span class="text-sm whitespace-nowrap">{{ tech.label }}</span>
            </div>
          </div>

          <div v-if="stack.type === 'single'" class="flex flex-col gap-2 items-center p-4 hover:shadow-custom-shadow-primary duration-150 hover:-translate-y-1.5">
            <img :src="stack.techs?.[0]?.icon" :alt="stack.techs?.[0]?.label" class="w-16" />
            <span>{{ stack.techs?.[0]?.label }}</span>
          </div>
        </div>


      </div>
      <button v-if="isMobile && techStacksDisplayed.length !== techStacks.length" @click="() => techStacksDisplayed = techStacks" class="text-primary font-semibold duration-150 border-b-2 border-secondary hover:text-primary hover:border-primary hover:-translate-y-1.5">Show more</button>
    </div>
  </section>
</template>
<script setup lang="ts">

type TechStacksType = {
  type: 'single' | 'multiple'
  techs: {
    label: string
    icon: string
  }[]
}

const techStacks: TechStacksType[] = [
  {
    type: 'multiple',
    techs: [
      {
        label: 'HTML5',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/html5/html5-original.svg'
      },
      {
        label: 'CSS3',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/css3/css3-original.svg'
      }
    ]
  },
  {
    type: 'multiple',
    techs: [
      {
        label: 'JavaScript',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/javascript/javascript-original.svg'
      },
      {
        label: 'TypeScript',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/typescript/typescript-original.svg'
      }
    ]
  },
  {
    type: 'multiple',
    techs: [
      {
        label: 'VueJS',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vuejs/vuejs-original.svg'
      },
      {
        label: 'NuxtJS',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nuxtjs/nuxtjs-original.svg'
      }
    ]
  },
  {
    type: 'multiple',
    techs: [
      {
        label: 'React',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/react/react-original.svg'
      },
      {
        label: 'NextJS',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nextjs/nextjs-plain.svg'
      }
    ]
  },
  {
    type: 'multiple',
    techs: [
      {
        label: 'Tailwind CSS',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/tailwindcss/tailwindcss-original.svg'
      },
      {
        label: 'Sass',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/sass/sass-original.svg'
      }
    ]
  },
  {
    type: 'multiple',
    techs: [
      {
        label: 'Node JS',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nodejs/nodejs-original-wordmark.svg'
      },
      {
        label: 'Express',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/express/express-original.svg'
      }
    ]
  },
  {
    type: 'multiple',
    techs: [
      {
        label: 'Java',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/java/java-original.svg'
      },
      {
        label: 'Spring',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/spring/spring-original.svg'
      }
    ]
  },
  {
    type: 'multiple',
    techs: [
      {
        label: 'MongoDB',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/mongodb/mongodb-original-wordmark.svg'
      },
      {
        label: 'PostgreSQL',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/postgresql/postgresql-original.svg'
      }
    ]
  },
  {
    type: 'single',
    techs: [
      {
        label: 'Supabase',
        icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/supabase/supabase-original.svg'
      }
    ]
  }
]

const techStacksDisplayed = ref<TechStacksType[]>(techStacks)

const isMobile = ref(false)



const handleResize = () => {
  isMobile.value = window.innerWidth < 768
  if(isMobile.value) {
    techStacksDisplayed.value = techStacks.slice(0, 5)
    return
  }

  techStacksDisplayed.value = techStacks
}


const isSectionVisible = ref(false);
const techStackSectionEl = ref(null);

onMounted(() => {
  handleResize()
  window.addEventListener('resize', handleResize)

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if(entry.isIntersecting) {
        isSectionVisible.value = entry.isIntersecting;
      }
    })
  })

  if (techStackSectionEl.value) {
    observer.observe(techStackSectionEl.value)
  }

  onUnmounted(() => {
    if (techStackSectionEl.value) {
      observer.unobserve(techStackSectionEl.value)
    }
  })
})
</script>
