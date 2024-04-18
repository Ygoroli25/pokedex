<script></script>

<template>
  <div class="footer">
    <div class="conteiner-footer">
      <img src="../assets/meia-bola.svg" alt="" width="60px" height="60px" />
      <p>
        Desenvolvido por
        <a href="https://github.com/Ygoroli25 " target="_bank"
          >Ygor O. Cardoso</a
        >
      </p>
    </div>
  </div>
</template>

<style scoped>
.footer {
  position: fixed;
  bottom: 0;
  width: 100%;
  background-color: #fff;
}

.conteiner-footer {
  padding: 20px 0;
  max-width: 1024px;
  margin: 0 auto;
  display: flex;
  justify-content: end;
  position: relative;
}

.conteiner-footer img {
  position: absolute;
  top: -18px;
  left: calc(50% - 30px);
}

a {
  text-decoration: none;
}
</style>
