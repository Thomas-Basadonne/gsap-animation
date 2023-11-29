<script setup>

// Metodo TO
// prende 2 parametri, il primo è per selezionare lelemento mentre il secondo è un oggetto ed è dove gestiamo lo tato dell'elemento. lo stato iniziale è quello definito dal nostro css
gsap.to('.square', {
  x: 200,
  })
</script>

<template>
  <h1 class="text-center bg-slate-200">Hello GSAP</h1>
  <!-- DIFFERNZA TRA TEEN E TIMELINE
  
      Tween: ci riferiamo ad una singola animazione, uno o piu elementi html che partono da uno stato iniziale A e arrivano ad uno stato finale B(ad esempio le classiche transizioni CSS). Metodi principali sono : .to() .from() .fromTo()

      Timeline: container di tween che ci permette di combinare insieme più animazioni su elementi diversi gestendone in maniera controllata i vari tempi, ritardi, le durate e il modo in cui si sincronizzano tra loro-->

  <!-- Animazioni metodo TO .to()-->
  <h2>Metodo TO</h2>
  <div class="bg-red-500 square"></div>
</template>


