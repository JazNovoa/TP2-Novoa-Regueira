<!-- Script JS -->
<!-- Script JS -->
<script>
  import * as d3 from "d3";
  import juegos from "/src/data/juegos.json";

  // Escala para la adictividad (color del relleno de los rectángulos st1)
  const adictividad = d3
    .scaleLinear()
    .domain([1, 2, 3, 4, 5])
    .range(["#000aff", "#9200b8", "#e92fed", "#ff9a51", "#ffe86f"]);

  // Escala para el continente (color del borde del path st0)
  const continente = d3
    .scaleOrdinal()
    .domain(["África", "Asia", "Europa", "America", "Oceania"])
    .range(["#000aff", "#9200b8", "#e92fed", "#ff9a51", "#ffe86f"]);

  // Función para cargar y modificar SVGs
  async function loadAndModifySVG(url, juego) {
    const response = await fetch(url);
    const svgText = await response.text();
    const parser = new DOMParser();
    const svgDoc = parser.parseFromString(svgText, "image/svg+xml");
    const svgElement = svgDoc.querySelector("svg");

    // Cambiar el color de los rectángulos con clase 'st1' según la adictividad
    svgElement.querySelectorAll(".st1").forEach((rect) => {
      rect.setAttribute("fill", adictividad(juego.Adictividad));
    });

    // Cambiar el color del path con clase 'st0' según el continente
    svgElement.querySelectorAll(".st0").forEach((path) => {
      path.setAttribute("stroke", continente(juego.Continente));
    });

    return svgElement.outerHTML;
  }

  // Cargar los SVGs con las modificaciones adecuadas
  let juegosSVG = [];

  $: (async () => {
    try {
      // Ordenar juegos por almacenamiento
      const juegosOrdenados = juegos.sort(
        (a, b) => a.Almacenamiento - b.Almacenamiento,
      );

      juegosSVG = await Promise.all(
        juegosOrdenados.map(async (juego) => {
          const svgContent = await loadAndModifySVG(juego.Imagen, juego);
          return { ...juego, svgContent };
        }),
      );
    } catch (error) {
      console.error(error);
    }
  })();
</script>

<!-- Estructura contenido HTML -->
<main>
  <div class="header">
    <img class="titulo" src="./public/images/TITULO.svg" width="50%" />
  </div>
<div class="contenedor-linea">
  <div class="linea"></div>
</div>
<p class="gigas1">0,3 GB</p>

<p class="gigas2">0,6 GB</p>

<p class="gigas3">0,9 GB</p>

<p class="gigas4">1,2 GB</p>

  <div class="contenedor">
    {#each juegosSVG as juego}
      <div class="juego-item">
        <p class="nombre-juego">{juego.Juego}</p>
        <div class="modulos">
          {#each Array(juego.Años) as _}
            <div class="svg-wrapper">
              {@html juego.svgContent}
            </div>
          {/each}
        </div>
      </div>
    {/each}
  </div>

  <div class="footer-container">
    <img class="indice" src="./public/images/INDICE-11.svg" />
    <h1 class="footer">Elaborado por Jazmin Novoa y Julieta Regueira <br>Visualización de Datos <strong>UTDT</strong></h1>
  </div>
</main>

<style>
@import url('https://fonts.googleapis.com/css2?family=Source+Code+Pro:ital,wght@0,200..900;1,200..900&display=swap');

  /* Estilos generales */
  .titulo,
  .header {
    padding-top: 50px;
    display: flex;
    justify-content: center;
    text-align: center;
    margin-bottom: 10px;
  }

  .footer-container {
  display: flex; 
  flex-direction: column; 
  align-items: center; 
  justify-content: center; 
  margin-top: 90px; 
  padding-bottom: 20px; 
}

.indice {
  width: 80%; 
  max-width: 700px; 
}

.footer {
  font-size: 15px;
  text-align: center; 
  margin-top: 80px;
  font-family: "Source Code Pro"; 
}


  main {
    height: 100%;
    margin: 0;
    background-color: black;
    color: white;
  }

  .contenedor {
    display: flex;
    flex-direction: column;
    max-width: 1020px;
    margin-left: 100px;
    align-items: flex-start;
    padding-top: 95px;
    position: relative;
  }

  .juego-item {
    display: flex;
    align-items: center;
    justify-content: flex-start; /* Alinear a la izquierda */
    margin: 0; /* Sin margen entre filas */
  }

  .modulos {
    display: flex;
    justify-content: flex-start; /* Alinear módulos a la izquierda */
    align-items: flex-start;
  }

  .svg-wrapper {
    display: block;
    justify-content: left;
    width: 60px;
    height: 60px;
  }

  .nombre-juego {
    width: 150px; /* Ajustar según sea necesario */
    text-align: right; /* Alinear el texto a la derecha */
    font-family: "Source Code Pro";
    font-size: 16px;
    color: white;
    margin: 0; /* Sin margen adicional */
    padding: 0; /* Sin padding */
    margin-right: 10px; /* Espacio entre el nombre del juego y los módulos SVG */
  }

  .contenedor-linea{
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 585px;
    height: 1000px;
    margin-left: 1320px;
    position: absolute;
  }

  .linea {
    border-left: 2px solid white !important;
    height: 2080px !important;
    margin-left: 100px;
  }

  .gigas1{
    width: 150px;
    text-align: right;
    font-size: 16px;
    font-family: "Source Code Pro";
    color: white;
    margin: 0;
    margin-top: 735px;
    margin-left: 1240px;
    padding: 0;
    margin-right: 10px; 
    position: absolute;
  }
  
  
  .gigas2{
    width: 150px;
    text-align: right;
    font-size: 16px;
    color: white;
    margin: 0;
    margin-top: 1210px;
    margin-left: 1240px;
    font-family: "Source Code Pro";
    padding: 0;
    margin-right: 10px; 
    position: absolute;
  }



  .gigas3{
    width: 150px;
    text-align: right;
    font-size: 16px;
    font-family: "Source Code Pro";
    color: white;
    margin: 0;
    margin-top: 1575px;
    margin-left: 1240px;
    padding: 0;
    margin-right: 10px; 
    position: absolute;
  }


  .gigas4{
    width: 150px;
    text-align: right;
    font-size: 16px;
    font-family: "Source Code Pro";
    color: white;
    margin: 0;
    margin-top: 1758px;
    margin-left: 1240px;
    padding: 0;
    margin-right: 10px; 
    position: absolute;
  }

</style>
