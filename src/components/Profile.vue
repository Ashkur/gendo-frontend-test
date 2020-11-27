<template>
  <div class="profile">
    <div class="container">
      <div class="row">
        <div class="profile__user">
          <div
            class="profile__image"
            :style="`background-image: url(${user.avatar_url})`"
          ></div>
          <div class="profile__user-info">
            <strong class="profile__name">{{ user.name }}</strong>
            <p class="profile__bio">{{ user.bio }}</p>
          </div>
        </div>

        <div style="max-width: 279.5px">
          <div class="profile__projects">
            <button
              @click="handleSelected('repos')"
              class="profile__projects-types"
              :class="{
                'profile__projects-types--selected': selectedType === 'repos',
              }"
              type="button"
            >
              Repos <span>{{ repos.length }}</span>
            </button>
            <button
              @click="handleSelected('starreds')"
              class="profile__projects-types"
              :class="{
                'profile__projects-types--selected':
                  selectedType === 'starreds',
              }"
              type="button"
            >
              Starred <span>{{ starreds.length }}</span>
            </button>
          </div>

          <div class="input-search">
            <img src="@/assets/img/icons/search.svg" alt="Ícone de busca" />
            <input
              v-model="searchText"
              type="text"
              name="search"
              placeholder="filter by name"
              @keyup.enter="filteProjects"
            />
          </div>

          <div
            class="project"
            v-for="(project, index) in selectedProjects"
            :key="`repo-${index}`"
          >
            <a
              :href="project.html_url"
              target="_blank"
              rel="noopener noreferrer"
              class="project__title"
            >
              <h3 v-if="selectedType === 'repos'">{{ project.name }}</h3>
              <div v-else>
                <h3>
                  <span class="font-normal">
                    {{ nameAsArray(project.full_name)[0] }}
                    /
                  </span>
                  <span>
                    {{ nameAsArray(project.full_name)[1] }}
                  </span>
                </h3>
              </div>
            </a>

            <p class="project__description">
              {{ project.description }}
            </p>

            <div class="project__actions-container">
              <div class="project__meta">
                <template v-if="selectedType === 'repos'">
                  <svg
                    height="16"
                    viewBox="0 0 16 16"
                    version="1.1"
                    width="16"
                    aria-hidden="true"
                  >
                    <path
                      fill-rule="evenodd"
                      style="fill: #5c646d"
                      d="M4.72 3.22a.75.75 0 011.06 1.06L2.06 8l3.72 3.72a.75.75 0 11-1.06 1.06L.47 8.53a.75.75 0 010-1.06l4.25-4.25zm6.56 0a.75.75 0 10-1.06 1.06L13.94 8l-3.72 3.72a.75.75 0 101.06 1.06l4.25-4.25a.75.75 0 000-1.06l-4.25-4.25z"
                    ></path>
                  </svg>
                  {{ project.language ? project.language : "Indefinido" }}
                </template>

                <template v-else>
                  <svg
                    class="octicon octicon-star"
                    viewBox="0 0 16 16"
                    version="1.1"
                    width="16"
                    height="16"
                    aria-hidden="true"
                  >
                    <path
                      fill-rule="evenodd"
                      style="fill: #5c646d"
                      d="M8 .25a.75.75 0 01.673.418l1.882 3.815 4.21.612a.75.75 0 01.416 1.279l-3.046 2.97.719 4.192a.75.75 0 01-1.088.791L8 12.347l-3.766 1.98a.75.75 0 01-1.088-.79l.72-4.194L.818 6.374a.75.75 0 01.416-1.28l4.21-.611L7.327.668A.75.75 0 018 .25zm0 2.445L6.615 5.5a.75.75 0 01-.564.41l-3.097.45 2.24 2.184a.75.75 0 01.216.664l-.528 3.084 2.769-1.456a.75.75 0 01.698 0l2.77 1.456-.53-3.084a.75.75 0 01.216-.664l2.24-2.183-3.096-.45a.75.75 0 01-.564-.41L8 2.694v.001z"
                    ></path>
                  </svg>
                  {{ project.stargazers_count }}
                </template>
              </div>

              <div class="project__meta" style="margin-left: 9px">
                <svg
                  viewBox="0 0 16 16"
                  version="1.1"
                  width="16"
                  height="16"
                  aria-hidden="true"
                >
                  <path
                    fill-rule="evenodd"
                    style="fill: #5c646d"
                    d="M11.75 2.5a.75.75 0 100 1.5.75.75 0 000-1.5zm-2.25.75a2.25 2.25 0 113 2.122V6A2.5 2.5 0 0110 8.5H6a1 1 0 00-1 1v1.128a2.251 2.251 0 11-1.5 0V5.372a2.25 2.25 0 111.5 0v1.836A2.492 2.492 0 016 7h4a1 1 0 001-1v-.628A2.25 2.25 0 019.5 3.25zM4.25 12a.75.75 0 100 1.5.75.75 0 000-1.5zM3.5 3.25a.75.75 0 111.5 0 .75.75 0 01-1.5 0z"
                  ></path>
                </svg>
                {{ project.forks }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    user: {
      type: Object,
      required: true,
    },
    repos: {
      type: Array,
      required: true,
    },
    starreds: {
      type: Array,
      required: true,
    },
  },

  data() {
    return {
      selectedType: "repos",
      selectedProjects: [],
      searchText: "",
    };
  },

  mounted() {
    this.selectedProjects = this.repos;
  },

  methods: {
    handleSelected(type) {
      if (type === "repos") {
        this.selectedType = type;
        this.selectedProjects = this.repos;
      } else {
        this.selectedType = type;
        this.selectedProjects = this.starreds;
      }
    },

    filteProjects() {
      const projects =
        this.selectedType === "repos" ? this.repos : this.starreds;

      const results = projects.filter(
        (project) =>
          project.full_name.toLowerCase().includes(this.searchText) ||
          (project.description &&
            project.description.toLowerCase().includes(this.searchText))
      );

      this.selectedProjects = results;
    },

    nameAsArray(projectName) {
      return projectName.split("/");
    },
  },
};
</script>

<style lang="scss">
.profile {
  margin-top: 13.5px;

  &__user {
    @apply flex items-center w-full justify-center;
  }

  &__user-info {
    line-height: 12px;
  }

  &__image {
    flex: 0 0 36.5px;
    margin-right: 5px;
    min-height: 36.5px;

    @apply rounded-full bg-no-repeat bg-center bg-cover;
  }

  &__name {
    font-size: 10px;
  }

  &__bio {
    @apply leading-tight text-gray-300;
  }

  &__projects {
    margin-top: 17.5px;
    @apply w-full flex items-center;
  }

  &__projects-types {
    padding: 8.5px 0;
    font-size: 9px;
    @apply w-6/12 text-gray-500 flex items-center justify-center relative;

    &:focus {
      @apply outline-none;
    }

    &::after {
      content: "";
      height: 2.5px;
      @apply w-0 transform transition-all duration-300 bg-orange absolute bottom-0 left-0;
    }

    span {
      flex: 0 0 13px;
      border-radius: 6.5px;
      margin-left: 5px;
      line-height: 1.1;
      @apply bg-gray-600 inline-block font-bold;
    }

    &--selected {
      @apply font-bold text-black-100;

      &::after {
        @apply w-full;
      }
    }
  }
}

.input-search {
  border-radius: 2px;
  padding: 4.5px 6px 2.5px;
  border-width: 0.5px;
  @apply flex border-gray-800 mt-4 w-full;

  img {
    max-width: 9px;
  }

  input {
    margin-left: 5px;
    @apply w-full;
  }
}

.project {
  padding: 7.5px 0 10.5px;
  @apply w-full;

  &__title {
    font-size: 10px;
    margin-bottom: 3px;
    @apply text-blue font-bold transition-colors duration-300;

    &:hover {
      @apply text-blue-100;
    }
  }

  &__description {
    font-size: 6px;
    margin-bottom: 6px;
    @apply text-gray-400;
  }

  &__actions-container {
    @apply flex items-center w-full;
  }

  &__meta {
    @apply flex items-center;

    svg {
      max-width: 8px;
      margin-right: 3px;
    }
  }
}

@media (max-width: 640px) {
  .profile {
    &__projects-types {
      border-bottom: solid 0.5px #e3e3e3;
    }
  }
}

@media (min-width: 640px) {
  .profile {
    &__user {
      @apply justify-start;
    }

    &__projects {
      border-bottom-width: 0.5px;
      @apply border-gray-700;
    }

    &__projects-types {
      max-width: 77px;
    }
  }

  .input-search {
    max-width: 144.5px;
  }
}

@media (min-width: 1024px) {
  .profile {
    &__user {
      flex: 0 0 144.5px;
      width: 144.5px;
      @apply justify-start items-center flex-col;
    }

    &__image {
      flex: 0 0 108.5px;
      width: 108.5px;
      height: 108.3px;
    }

    &__user-info {
      margin-top: 9px;
      @apply text-center;
    }

    &__projects {
      @apply mt-0;
    }

    &__bio {
      max-width: 108.5px;
    }
  }
}
</style>