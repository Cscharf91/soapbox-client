<style>
  .card {
    width: 30%;
    margin: 10px auto;
    background: #fff;
    border-radius: 6px;
    padding: 30px;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
    grid-column: 2;
  }

  @media(max-width: 800px) {
    .card {
      width: 100%;
      margin: 10px 0px;
    }
  }
</style>

<div class="card">
  <slot />
</div>