<script>
  import * as d3 from "d3"
  import apps from "/src/data/App.csv"
  import apps1 from "/src/data/AppJ.csv"
  // import atletas from "/src/data/athletes.json"

  console.log(apps);
  console.log(apps1);

  let svg;
  let tooltipVisible = false;
  let tooltipX = 10;
  let tooltipY = 0;
  let tooltipName = "";

  /* 1. Escala para participaciones Fede*/
  const minMaxAlmacenamiento = d3.extent(apps, (d) => +d.Almacenamiento);
  let grosorPartic = d3
    .scaleLinear()
    .domain(minMaxAlmacenamiento)
    .range([3, 12]); // Grosor de 1 a 3

  let tamañoPunta = d3
    .scaleLinear()
    .domain(minMaxAlmacenamiento)
    .range([7, 22]); // Tamaño de la punta de 5 a 20

  /* 1. Escala para participaciones Fede*/
  const minMaxAlmacenamiento1 = d3.extent(apps, (d) => +d.Almacenamiento);
  let grosorPartic1 = d3
    .scaleLinear()
    .domain(minMaxAlmacenamiento1)
    .range([1, 10]); // Grosor de 1 a 3

  let tamañoPunta1 = d3
    .scaleLinear()
    .domain(minMaxAlmacenamiento1)
    .range([5, 20]); // Tamaño de la punta de 5 a 20

  /* 3. Escala para Me gusta (Base, despues se le da la saturacion, en colorMe_gusta) */
  const colorMe_gusta_base = d3
    .scaleOrdinal()
    .domain(["Si", "No"])
    .range(["#32FF00", "#FF0000"]); // Verde para "Si", Rojo para "No"

  // Escala para la saturación según el uso
  const saturacionUso = d3.scaleOrdinal()
    .domain(["Lo uso", "A veces", "No lo uso"])
    .range([1, 0.6, 0.3])

  // Función para aplicar saturación al color base
  function colorMe_gusta(gusta, uso){
    let cgusta=0
    if (gusta == 'Si'){
      cgusta=100
    }
    let cuso=100
    if (uso =='A veces'){
      cuso=60
    }
    else if (uso =='No lo Uso'){
      cuso=30
    }
    return `hsl(${cgusta} ${cuso} 50% / 1)`
  }

  /* 4. Escala para Tipo */
  const colorTipo = d3.scaleOrdinal()
    .domain(["Musica", "Red Social", "Fotos y Videos", "Servicio", "Entretenimiento", "Juego"])
    .range(["#B200FF", "#45D2F1", "#FFA600", "#FF00D4", "#0009FF", "#9F4343"])
  
  const centerX = 300;
  const centerY = 300;
  const radius = 200;

  // Ángulo de rotación manual
  let manualRotation = 270;

  function angle(i) {
    return (i / apps.length) * (2 * Math.PI);
  }

  function x1(i) {
    return centerX + Math.cos(angle(i)) * radius
  }

  function y1(i) {
    return centerY + Math.sin(angle(i)) * radius
  }

// Función para calcular las coordenadas de la punta
function punta(x, y, tipo, almacenamiento) {
    const size = tamañoPunta(almacenamiento); // Tamaño de la punta basado en el almacenamiento
    const color = colorTipo(tipo);

    return `
      ${x},${y} 
      ${x - size},${y - size} 
      ${x + size},${y - size}
    `;
  }
  function punta1(x, y, tipo, almacenamiento) {
    const size = tamañoPunta1(almacenamiento); // Tamaño de la punta basado en el almacenamiento
    const color = colorTipo(tipo);

    return `
      ${x},${y} 
      ${x - size},${y - size} 
      ${x + size},${y - size}
    `;
  }
// Función para calcular la rotación
function rotacion(index) {
    const ang = angle(index) + manualRotation * (Math.PI / 180); // Añadir la rotación manual
    return `rotate(${(ang * 180) / Math.PI}, ${x1(index)}, ${y1(index)})`;
  }

  function showTooltip(event, name) {
    tooltipX = event.pageX +15;
    tooltipY = event.pageY;
    tooltipName = name;
    tooltipVisible = true;
  }

  function hideTooltip() {
    tooltipVisible = false;
  }
</script>

<main>
  <div class="header">
    <!-- <img src="/images/logo_referencias.svg" width="190" alt="anillos" /> -->
    <h3 class="headline">
      <b>Brillando en cada pantalla con apps estelares</b>
      <!-- <b>Las estrellas que brillan en cada frame</b> -->
      <!-- <b>Las reinas de mi pantalla</b> -->
    </h3>
    <p class="bajada">
      Uso y almacenamiento de las apps más populares
    </p>
    <div class="container">
      <div class = "Fede">
        <h4 style="text-align: center; margin-bottom: -50px; margin-top: 60px">Federico</h4>
        <svg id="visualization" width="600" height="600">
          {#each apps as app, index}
            <line
              x1={centerX}
              y1={centerY}
              x2={x1(index)}
              y2={y1(index)}
              stroke={colorMe_gusta(app.Me_gusta, app.Uso)}
              stroke-width={grosorPartic(app.Almacenamiento)}
            ></line>
            <polygon
                points={punta(x1(index), y1(index) + 5, app.Tipo, app.Almacenamiento)}
                fill={colorTipo(app.Tipo)}
                transform={rotacion(index)}
                on:mouseover={(e) => showTooltip(e, app.Nombre)}
                on:mousemove={(e) => showTooltip(e, app.Nombre)}
                on:mouseout={hideTooltip}
              ></polygon>
            {/each}
            <image 
              href="/images/Celular.svg"  
              x={centerX - 47}  
              y={centerY - 50}  
              width="100"  
              height="100"
            />
        </svg>
      </div>
      <div class = "Jony">
        <h4 style="text-align: center; margin-bottom: -50px; margin-top: 60px">Jonathan</h4>
        <svg id="visualization" width="600" height="600">
          {#each apps1 as app1, index}
            <line
              x1={centerX}
              y1={centerY}
              x2={x1(index)}
              y2={y1(index)}
              stroke={colorMe_gusta(app1.Me_gusta, app1.Uso)}
              stroke-width={grosorPartic(app1.Almacenamiento)}
            ></line>
            <polygon
                points={punta(x1(index), y1(index) + 5, app1.Tipo, app1.Almacenamiento)}
                fill={colorTipo(app1.Tipo)}
                transform={rotacion(index)}
                on:mouseover={(e) => showTooltip(e, app1.Nombre)}
                on:mousemove={(e) => showTooltip(e, app1.Nombre)}
                on:mouseout={hideTooltip}
              ></polygon>
            {/each}
            <image 
              href="/images/Celular.svg"  
              x={centerX - 47}  
              y={centerY - 50}  
              width="100"  
              height="100"
            />
        </svg>
      </div>
    </div>
    <br>
    <br>
    <br>
    <br>
    <div class="Imagenes" style="margin: 5px; padding: 8px; margin-top: -70px">    
      <img
        class="referencias"
        src="/images/Almacenamiento1.svg"
        alt="almacenamiento"
      />
      <img 
      class="referencias"
      src="/images/Gusto1.svg"
      alt="gusto"
      />
      <img 
      class="referencias"
      src="/images/Uso1.svg"
      alt="uso"
      />
      <img 
      class="referencias"
      src="/images/Genero1.svg"
      alt="genero"
      />
  </div>

  <div class="Foot" style="margin-top: -30px;">
  <footer class="footer">
    <p style= "font-size: 12px; margin-bottom: 5px ">Creado por Federico Villanueva y Jonathan Jeifetz</p>
    <p style= "font-size: 12px; margin-bottom: 5px">
      <a href="https://www.linkedin.com/in/federico-mateo-villanueva-a52196279" target="_blank">LinkedIn</a> |
      <a href="https://github.com/fedemvilla/vd_maqueta-main" target="_blank">GitHub</a>
    </p>
    <p style="font-size: 12px; margin-bottom: 5px">
      <a href="https://mail.google.com/mail/?view=cm&fs=1&to=fvillanueva@mail.utdt.edu" target="_blank">fvillanueva@mail.utdt.edu</a> |
      <a href="https://mail.google.com/mail/?view=cm&fs=1&to=jjeifetz@mail.utdt.edu" target="_blank">jjeifetz@mail.utdt.edu</a> 
    </p>
    <div class="DiTella">
      <p style= "font-size: 12px; margin-bottom: 20px;">Visualización de Datos, Universidad Torcuato Di Tella</p>
    </div>
  </footer>
</div>
  {#if tooltipVisible}
    <div class="tooltip" style="left: {tooltipX}px; top: {tooltipY}px;">
      {tooltipName}
    </div>
  {/if}
</main>

<style>
  .tooltip {
    position: absolute;
    background-color: black;
    color: white;
    padding: 5px;
    border-radius: 5px;
    pointer-events: none;
  }
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
    font-size: 22px;
    font-weight: 300;
    text-align: center;
    margin: 10px;
  }

  .container {
    display: flex;
    justify-content: center;
    margin-top: 20px;
    margin-bottom: -60px;
  }

  .Imagenes {
    display: flex;                /* Usamos flexbox para alinear las imágenes */
    justify-content: center;      /* Centra las imágenes horizontalmente */
    align-items: center;          /* Centra las imágenes verticalmente */
    gap: 15px;                   /* Espaciado entre las imágenes */
    flex-wrap: wrap;         /* Permite que las imágenes se ajusten en varias filas si es necesario */
  }

  .referencias {
    width: 285px;                /* Ancho ajustado para las imágenes */
    height: 285px;               /* Altura ajustada para las imágenes */
    object-fit: contain;         /* Ajusta la imagen dentro del área sin recortarla */
  }

  .footer a {
    color: #0077b5;  /* Color para los enlaces */
    text-decoration: none;
    margin: 0 3px;
  }

  .footer {
    text-align: center;
    color: gray;  /* Color gris clarito */
    padding: 20px;
  }

  .footer a:hover {
    color: gray;  /* Cambia a un gris más oscuro al pasar el ratón por encima */
  }

</style>
