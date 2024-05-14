<template>
  <div class="articles-wrapper">
    <div class="article">
      <div>
        <span class="title">Lorem ipsum</span>
        <p class="text">
          Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam
          nonummy nibh euismod tincidunt ut laoreet dolore magna aliquam erat
          volutpat. Ut wisi enim ad minim veniam, quis nostrud exerci tation
          ulla- mcorper suscipit lobortis nisl ut aliquip ex ea commodo
          consequat. Duis autem vel eum iriure dolor in hendrerit in vulputate
          velit esse molestie consequat, vel illum dolore eu feugiat nulla
          facilisis at vero
        </p>
        <button class="link">Lorem ipsum</button>
      </div>
      <div class="image" />
    </div>
    <div class="article">
      <div class="image" />
      <div>
        <span class="title">Lorem ipsum</span>
        <p class="text">
          Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam
          nonummy nibh euismod tincidunt ut laoreet dolore magna aliquam erat
          volutpat. Ut wisi enim ad minim veniam, quis nostrud exerci tation
          ulla- mcorper suscipit lobortis nisl ut aliquip ex ea commodo
          consequat. Duis autem vel eum iriure dolor in hendrerit in vulputate
          velit esse molestie consequat, vel illum dolore eu feugiat nulla
          facilisis at vero
        </p>
        <button class="link">Lorem ipsum</button>
      </div>
    </div>
  </div>
</template>

<style lang="sass">
.articles-wrapper
  max-width: 1200px
  margin: 2rem auto
  display: flex
  flex-direction: column

.article
  margin: 2rem
  display: flex
  gap: 4rem

  .title
    font-size: 3rem
    font-weight: 700

  .link
    background-color: var(--secondary-color)
    padding: .5rem 3rem .5rem 3rem
    font-size: 1rem

  .image
    width: 250rem
    background-color: var(--secondary-color)

  .text
    line-height: 1.2rem
</style>
