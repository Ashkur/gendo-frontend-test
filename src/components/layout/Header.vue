<template>
  <div class="header">
    <div class="container">
      <div class="header__info">
        <img
          class="header__logo"
          src="@/assets/img/logos/github.png"
          alt="Logo Github"
        />
        <div class="header__title"><strong>Github</strong> profiles</div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.header {
  min-height: 26.5px;
  padding: 6.5px 11.5px;
  @apply bg-black w-full;

  &__info {
    @apply flex items-center;
  }

  &__logo {
    max-width: 13.5px;
    margin-right: 5px;
  }

  &__title {
    font-size: 10px;
    @apply text-white;
  }
}
</style>