<script>
  import * as d3 from "d3"
  import apps from "/src/data/App.csv"
  // import atletas from "/src/data/athletes.json"

  console.log("apps", apps)

  /* 1. Escala para participaciones (cuantitativo > grosor) */
  const minMaxAlmacenamiento = d3.extent(apps, (d) => d.Almacenamiento)
  let grosorPartic = d3.scaleLinear()
    .domain(minMaxAlmacenamiento)
    .range([1, 500]) // O de 1 a 3
  
  /* 3. Escala para genero (categórico > color) */
  const colorMe_gusta_base = d3.scaleOrdinal()
    .domain(["Si", "No"])
    .range(["#32FF00", "#FF0000"])

  // Escala para la saturación según el uso
  const saturacionUso = d3.scaleOrdinal()
    .domain(["Lo uso", "A veces", "No lo uso"])
    .range([1, 0.6, 0.3])

  // Función para aplicar saturación al color base
  const colorMe_gusta = (gusta, uso) => {
    let colorBase = colorMe_gusta_base(gusta) // Obtiene el color base (verde o rojo)
    let saturacion = saturacionUso(uso) // Obtiene la saturación (1, 0.6 o 0.3)

    return d3.color(colorBase).brighter(saturacion); // Ajusta la saturación
  }

  /* 4. Escala para continentes (categórico > color)   */
  const colorTipo = d3.scaleOrdinal()
    .domain(["Musica", "Red Social", "Foto y Video", "Servicio", "Entretenimiento", "Juego"])
    .range(["#B200FF", "#FFEE00", "#FFA600", "#FF00D4", "#0009FF", "#9F4343"])

  /* 4. Escala para continentes (categórico > color)   */
    // const colorTipo = d3.scaleOrdinal()
    //   .domain(["Musica", "Red Social", "Foto y Video", "Servicio", "Entretenimiento", "Juego"])
    //   .range(["#B200FF", "#FFEE00", "#FFA600", "#FF00D4", "#0009FF", "#9F4343"])

</script>

<main>
  <div class="header">
    <img src="/images/logo_referencias.svg" width="190" alt="anillos" />
    <h3 class="headline">
      <b>Los reyes del oro</b>
      Medallas, participaciones y dominio en distintos continentes
    </h3>
    <p class="bajada">
      Los atletas con más medallas olímpicas de oro en los Juegos Olímpicos
    </p>
    <img
      class="referencias"
      src="/images/referencias.svg"
      width="490"
      alt="anillos"
    />
  </div>

    <!-- Conedor de las entidades -->
    <div class="container">
      
      <!-- Iteramos la data para visualizar c/ entidad -->
      <!-- {#each atletas as atleta}
        <div class="person-container">
          <div
            class="person"
            style="
              border-color: {colorContinentes(atleta.continent)};
              background-color:{colorGenero(atleta.gender)}; 
              width: {diamMedallas(atleta.medallas)}px; 
              height: {diamMedallas(atleta.medallas)}px; 
              border-width: {grosorPartic(atleta.participations)}px; 
            ">
          </div>
        </div>
      {/each} -->
      <!-- Fin iteración -->

    </div>
</main>

<style>
  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-top: 20px;
    margin-bottom: 80px;
  }
  .headline {
    font-size: 40px;
    font-weight: 300;
    line-height: 1.2;
    text-align: center;
    margin: 20px;
  }
  .bajada {
    font-size: 24px;
    font-weight: 300;
    text-align: center;
    margin: 10px;
  }
  .headline b {
    display: block;
  }
  .container {
    display: flex;
    justify-content: center;
    align-items: end;
    margin: auto;
    flex-wrap: wrap;
    max-width: 1020px;
    gap: 30px;
    margin-bottom: 100px;
  }
  .person-container {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    flex: 180px 0 0;
  }
  .person {
    width: 100px;
    height: 100px;
    border: 10px solid black;
    border-radius: 50%;
    box-sizing: border-box;
    background-color: pink;
  }
  .nombre {
    font-size: 13px;
    font-weight: bold;
    line-height: 1;
    text-transform: uppercase;
    margin: 0;
    margin-top: 8px;
  }
  .deporte {
    font-size: 14px;
    color: #666;
    margin: 0;
  }
  .referencias {
    margin-top: 50px;
    margin-bottom: 20px;
  }
</style>
