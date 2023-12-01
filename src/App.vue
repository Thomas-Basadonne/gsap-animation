<script setup>
import { onMounted } from "vue";
import { gsap } from "gsap";

// Metodo TO
// prende 2 parametri, il primo è per selezionare lelemento mentre il secondo è un oggetto ed è dove gestiamo lo tato dell'elemento. lo stato iniziale è quello definito dal nostro css
//Metodo FROM funziona nel modo opposto di TO, nel metodo TO definiamo lo stato finale nel metodo FROM lo stoto iniziale. quindi gsap prenderà le proprietà iniziali date al nostro elemento e le trasformerà nelle proprietà finali e quelle date al metodo .from() diventeranno le iniziali
onMounted(() => {
  const square = document.querySelector(".square");
  square.addEventListener("click", () => {
    //dopo la prima animazione se clicco non si ripete perche con il metodo .to() definiamo lo stato finale dell'elemento e quindi una volta eseguita la prima volta è gia allo stato finale ed è per questo che non si vedono cambiamenti
    gsap.to(".square", {
      x: 400,
      delay: 0.5,
      duration: 3,
      opacity: 1,
    });
  });

  // un altro modo per selezionare gli elementi oltre che con i selttori css
  //recupero tutti gli elementi che hanno classe .square
  // const squares = document.querySelectorAll('.square')
  // gsap.to(squares, {
  //   duration:4,
  //   opacity:0,
  //   y:100
  // })

  // Utilizzioamo un array per selezionare tutti gli elementi
  gsap.to([".square", ".rect", ".circle"], {
    duration: 4,
    opacity: 0,
    y: 100,
    rotate: 180,
    borderRadius: "50%",
  });

  //FROM
  // tip: quando si lavora con opacity è buono usare autoAlpha invece che opacity, perche altrimenti l'elemento invisibile se posizionato ad esempio sopra dei link non ci permetterà di cliccarli. autoAlpha invece nel momento in cui l'opacità è 0 mette visibilità hidden cosi da non influire su altri elementi
  gsap.from([".square-f", ".rect-f", ".circle-f"], {
    duration: 4,
    delay: 0.8,
    autoAlpha: 0,
    x: -300,
  });

  //FROM TO combinazione dei 2 metodi, con questo metodo definiamo sia lo stato iniziale che lo stato finale dell'elemento passando 2 oggetti.
  gsap.fromTo(
    [".square-ft", ".rect-ft", ".circle-ft"],
    {
      autoAlpha: 0,
      x: 300,
      rotate: 180,
    },
    {
      //le proprietà tipiche delle animazioni quali durata, delay, ecc ecc vanno nel secondo oggetto
      autoAlpha: 1,
      x: 0,
      rotate: 0,
      duration: 4,
      delay: 1,
      ease: "linear",
    }
  );
  //TIMING FUNCTION
  // il segreto di ogni buona animazione è nel tepismo e nel timing velocita. gsap usa una timing function di default, ma è possibile modificarla. andando a lavorare sulla proprietà ease. .in .out e .inOut gestiscono la timing function, in se la vogliamo all'inizio, out alla fine e inOut a inizio e fine.
  document.querySelector(".start").addEventListener("click", () => {
    console.log("Button clicked!");
    gsap.to(".square-time", {
      x: 500,
      duration: 3,
      ease: "none", //velocità costante per tutta la durata dell'animazione
    });
    gsap.to(".rect-time", {
      x: 500,
      duration: 3,
      ease: "Power3.out",
    });
    gsap.to(".circle-time", {
      x: 500,
      duration: 3,
      ease: "bounce.in", //simula una collisione con rimbalzo
    });
  });

  //Metodi Set e Get  Property SET= ci permette di settare istantaneeamente uno stile css(add style inline) metodo GET chiede a un elem html uno qualsiasi dei suoi attributi css sia che siano inline che nel foglio di stile
  //GET
  //quando si specifica il target è uguale a un queryselector ovvero prende solo il primo elemento con quella classe e gli altri no
  const color = gsap.getProperty("#square", "backgroundColor"); // in questo caso ho usato bg color ma si puo chiedere tutto altezza, larghezza ecc posso aggiungere unita di misura ad esempio("#square", "width", 'px'). in automati questo metodo controlla prima se ci sono stili inlinea e se non ci sono va a vedere nel css. se come attributo di ricerca metti 'class' ti trova tutte le classi date all'elemento
  console.log(color); //in console ci compare il colore esatto dato all'elemento in rgb

  // possiamo creare un getter generico. cosi facendo abbiamo una funzione riutilizzabile per per ottenere una proprietà dell'elemento selezionato
  const getter = gsap.getProperty("#square");
  const x = getter("x"); //mi fornisce la traslazione
  const bg = getter("backgroundColor"); // mi fornisce il bg color
  const id = getter("id"); //mi da id

  //SET
  //ci permette di poter settare su più elementi dei valori css. ci semplifica molto soprattutto quando abbiam un insieme di elementi ai quali vogliamo impostare un valore iniziale. per esempio da usare come partenza a tutte le animazioni iniziali.
  gsap.set(["#square", "#circle", "#rect"], {
    autoAlpha: 0,
    x: -200,
  });
  gsap.to(["#square", "#circle", "#rect"], {
    autoAlpha: 1,
    duration: 3,
    x: 0,
    ease: "elastic",
  });

  //stagger animation
  //prorpieta che noi possiamo inserire nei tween quando animiamo una collezione di elementi simultaneamente. ci permette di sfalzare le singole animazioni facendo in modo che partano uno dall'altro con del delay
  //obbietivvo: far entrare i link header dall'alto con varie proprietà css
  gsap.set("header li", {
    autoAlpha: 0,
    y: -200,
  });
  document.querySelector(".mostra").addEventListener("click", () => {
    gsap.to("header li", {
      autoAlpha: 1,
      y: 0,
      duration: 2,
      ease: "bounce",
      // stagger: 0.1, //BASIC sfalza l'animazione di 0.1 secondi
      stagger: {
        // each: 0.1, each gestisce lo sfasamento, come nell'esempio sopra
        amount: 2, //cosi facendo dai il tempo totale e gsap calcola lo sfasamento custom
      },
    });
  });

  document.querySelector(".anima").addEventListener("click", () => {
    gsap.to(".cerchio", {
      y: 50,
      duration: 1,
      ease: "power3.out",
      repeat: -1, // va all'infinito, vedi sotto
      yoyo: true, //ripete lanimazione al contrario, vedi sotto
      stagger: {
        each: 0.1,
        from: "center", //da dove far partire l'animazione end start edges random o anche l'indice dell elemento da cui partire. se in una griglia puoi specifivare la riga la colonna o di capire lui come sono disposti
        // grid: [5, 11], rispettivamente righe e colonne se metti 'auto' fa lui
        //axis: 'y', per lavorare in base agli assi
        // ease: .1, in questo caso dentro lo stagger viene utilizzata per calcolare lo slittamento delle animazioni
        //avendole messe dentro lo stagger si ripete per ognuni elemento indipendentemente dal gruppo
        repeat: -1, // va all'infinito, vedi sotto
        yoyo: true, //ripete lanimazione al contrario, vedi sotto
      },
    });
  });

  // REPEAT E YOYO

  // repeat
  document.querySelector(".ripeti").addEventListener("click", () => {
    gsap.to(".square-repeat", {
      rotate: 180, // 'random(-180,180)' prende un valore randomico, interessante da usare con repeatRefresh
      //x:'+=50', ad ogni ripetizione aggiunge un numero di px, interessante da usare con repeatRefresh
      duration: 2,
      ease: "Power2.out",
      repeat: -1, //con repeat indichi il numero di volte che lelemento target dovra fare l'animazione, per farlo ripetere all'infinito mettere -1
      repeatDelay: 0.5, // questo gestisce il delay tra una ripetizione e l'altra
      yoyo: true, // con la modalità yoyo true la prima animazione sarà normale ma la seconda sarà al contrario per poi invertirsi ogni volta
      yoyoEase: "none", //questo gestisco lo ease della ripetizione al contrario
      // repeatRefresh: true, // con lui true Rigenera l'animazione prima di ogni iterazione, garantendo continuità.
    });
  });

  // CALLBACK funzione a tutti gli effetti che viene chiamata allo scatenarsi di un evento nei nostri tween e timeline. ad esempio possiamo evocare le f quando l'animazione comincia, finisce o in un momento specifico o anche ad ogni ripetizione se utilizzo repeat
  function onComplete(param1, param2) {
    console.log("completa", param1 + param2);
  }
  document.querySelector(".callback").addEventListener("click", () => {
    const tween = gsap.to(".callback-square", {
      duration: 2,
      x: 200,
      borderRadius: "100",
      repeat: 3, //per vedere onRepeat
      onRepeat: () => {
        // onRepeatParams per mandare parametri
        console.log("ripetuta");
      },
      // onReverseComplete(){//altro modo di chiamare una callback senza arrow function ma direttamente dalla funzone gsap
      //   //viene chiamata quando viene completato il reverse di una funzione
      // }
      //paused: true, //tween creato ma in pausa
      // onStart: () => {
      //   // quando inizia l'animazione, non quando viene creato il tween ad esempio in questo caso ho aggiunto un paused sopra
      //   //se devo passare params onCompleteParams
      //   console.log("inizia");
      // },
      // onComplete: onComplete, // qunado la animazione è finita, non viene invocata se il tween si ferma prima del completamento animazione.
      // onCompleteParams: [5, 10], //serve per passare dati alla funzione
      // // onComplete:()=>{
      // //   console.log('completa');
      // // },//possiamo passargli una function(),una arrov function o possiamo creare una funzione onComplete() esterna da richiamare. forse ha piu senso arrow function per il this
      // onInterrupt: () => {
      //   // chiama una funzione quando lanimazione viene interrotta
      //   //se devo passare params onInterruptParams
      //   console.log("interrotta");
      // },
      // onUpdate: () => {
      //   //invocata ad ogni aggiornamento della nostra animazione, anche qui cè onUpdateParams
      //   console.log("update");
      // },
    });
    // setTimeout(() => {
    //   //timer per testare onINterrupt
    //   tween.kill(); //metodo per eliminare un tween
    // }, 2000);
  });
});
</script>

<template>
  <h1 class="text-center bg-slate-200">Hello GSAP</h1>
  <!-- stagger -->
  <header>
    <nav class="container">
      <ul class="flex py-4 justify-end">
        <li class="px-8 leading-loose">home</li>
        <li class="px-8 leading-loose">servizi</li>
        <li class="px-8 leading-loose">info</li>
        <li class="px-8 leading-loose">contatti</li>
      </ul>
    </nav>
  </header>
  <div class="p-10">
    <button
      class="mostra bg-blue-500 hover:bg-blue-200 text-white py-1 px-2 m-2 rounded-md"
    >
      mostra header
    </button>
  </div>
  <!-- callback -->
  <div class="callback-square w-12 h-12 rounded-md bg-sky-300 ml-5"></div>
  <div class="p-10">
    <button
      class="callback bg-blue-500 hover:bg-blue-200 text-white py-1 px-2 m-2 rounded-md"
    >
      callback
    </button>
  </div>
  <!-- stagger -->
  <div class="container">
    <ul class="flex py-4 justify-between w-full text-center">
      <li class="cerchio h-8 w-8 rounded-full bg-green-100"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-200"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-300"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-400"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-500"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-600"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-700"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-600"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-500"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-400"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-300"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-200"></li>
      <li class="cerchio h-8 w-8 rounded-full bg-green-100"></li>
    </ul>
  </div>
  <div class="p-10">
    <button
      class="anima bg-blue-500 hover:bg-blue-200 text-white py-1 px-2 m-2 rounded-md"
    >
      anima
    </button>
  </div>
  <!-- repeat  -->
  <div class="flex justify-center">
    <div class="square-repeat bg-green-300 h-16 w-16"></div>
  </div>
  <div class="flex justify-center">
    <button
      class="ripeti bg-sky-500 hover:bg-blue-200 text-white py-1 px-2 m-2 rounded-md"
    >
      ripeti
    </button>
  </div>
  <!-- DIFFERNZA TRA TEEN E TIMELINE
  
      Tween: ci riferiamo ad una singola animazione, uno o piu elementi html che partono da uno stato iniziale A e arrivano ad uno stato finale B(ad esempio le classiche transizioni CSS). Metodi principali sono : .to() .from() .fromTo()

      Timeline: container di tween che ci permette di combinare insieme più animazioni su elementi diversi gestendone in maniera controllata i vari tempi, ritardi, le durate e il modo in cui si sincronizzano tra loro
   -->

  <!-- Animazioni metodo TO .to()-->
  <h2 class="my-5 py-5">Metodo TO</h2>
  <div class="flex gap-4">
    <div class="bg-red-500 square"></div>
    <div class="bg-violet-500 rect"></div>
    <div class="bg-yellow-500 circle"></div>
  </div>
  <!-- Animazioni metodo .from()-->
  <h2 class="my-5 py-5">Metodo FROM</h2>
  <div class="flex gap-4">
    <div class="bg-blue-500 square-f"></div>
    <div class="bg-amber-500 rect-f"></div>
    <div class="bg-red-200 circle-f"></div>
  </div>
  <!-- Animazioni metodo TO .fromTo()-->
  <h2 class="my-5 py-5">Metodo FROM TO</h2>
  <div class="flex gap-4">
    <div class="bg-pink-500 square-ft"></div>
    <div class="bg-slate-500 rect-ft"></div>
    <div class="bg-green-500 circle-ft"></div>
  </div>

  <!-- Timing function -->
  <h2 class="my-5 py-5">Timing function</h2>
  <div class="my-4">
    <div class="bg-pink-500 square-time my-2"></div>
    <div class="bg-slate-500 rect-time my-2"></div>
    <div class="bg-green-500 circle-time my-2"></div>
  </div>
  <button
    class="start bg-blue-500 hover:bg-blue-200 text-white py-1 px-2 m-2 rounded-md"
  >
    Start
  </button>
  <!-- SET & GET -->
  <h2 class="my-5 py-5">SET & GET</h2>
  <div class="my-4">
    <div id="square" class="bg-blue-300 w-52 h-52 my-2"></div>
    <div id="rect" class="bg-slate-500 w-72 h-40 my-2"></div>
    <div
      id="circle"
      class="bg-green-200 w-52 h-52 rounded-full my-2"
      style="opacity: 0.5"
    ></div>
  </div>
  <button
    class="start bg-blue-500 hover:bg-blue-200 text-white py-1 px-2 m-2 rounded-md"
  >
    Start
  </button>
</template>
