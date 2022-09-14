<div class="bg">
  <div class="loader">
    <div class="ring" />
    <div class="loading">Loading...</div>
  </div>
</div>

<style lang="scss" scoped>
  .bg {
    position: fixed;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;

    background: rgba(0, 0, 0, 0.5);

    display: flex;
    align-items: center;
    justify-content: center;
  }

  .loader {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .ring {
    display: inline-block;

    &::after {
      content: " ";
      display: block;

      max-width: 10rem;
      max-height: 10rem;

      width: 50vmin;
      aspect-ratio: 1;

      border-radius: 50%;
      border: 0.35rem solid #fff;
      border-color: #fff transparent #fff transparent;
      animation: ring-animation 1.2s linear infinite;
    }
  }

  .loading {
    font-weight: bold;
    margin-top: 1.25rem;
  }

  @keyframes ring-animation {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
</style>
