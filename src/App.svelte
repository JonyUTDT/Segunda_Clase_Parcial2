<script>
  import * as d3 from "d3"
  import apps from "/src/data/App.csv"
  import apps1 from "/src/data/AppJ.csv"
  // import atletas from "/src/data/athletes.json"

  console.log(apps);
  console.log(apps1);

  let svg;

  /* 1. Escala para participaciones Fede*/
  const minMaxAlmacenamiento = d3.extent(apps, (d) => +d.Almacenamiento);
  let grosorPartic = d3
    .scaleLinear()
    .domain(minMaxAlmacenamiento)
    .range([1, 10]); // Grosor de 1 a 3

  let tamañoPunta = d3
    .scaleLinear()
    .domain(minMaxAlmacenamiento)
    .range([5, 20]); // Tamaño de la punta de 5 a 20

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
</script>

<main>
  <div class="header">
    <!-- <img src="/images/logo_referencias.svg" width="190" alt="anillos" /> -->
    <h3 class="headline">
      <b>Brillando en cada pantalla con apps estelares</b>
      <!-- <b>Las estrellas que brillan en cada frame</b> -->
      <!-- <b>Las reinas de mi pantalla</b> -->
      <p style="font-size: 18px;">Uso y almacenamiento de las apps más populares</p>
    </h3>
    <p class="bajada">
      Las apps con más descargas y nuevos usuarios en los últimos 3 años
    </p>
    <br>
    <br>
    <div class="Imagenes" style="margin: 5px; border: 2px solid grey; padding: 8px;">    <img
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
  <div class="container">
    <div class = "Fede">
    <h4 style="text-align: center; margin-bottom: -50px; margin-top: 80px">Federico</h4>
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
    <h4 style="text-align: center; margin-bottom: -50px; margin-top: 80px">Jonathan</h4>
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
    font-size: 22px;
    font-weight: 300;
    text-align: center;
    margin: 10px;
  }

  .container {
    display: flex;
    justify-content: center;
    margin-top: 20px;
  }

  .Imagenes {
    display: flex;                /* Usamos flexbox para alinear las imágenes */
    justify-content: center;      /* Centra las imágenes horizontalmente */
    align-items: center;          /* Centra las imágenes verticalmente */
    gap: 15px;                   /* Espaciado entre las imágenes */
    flex-wrap: wrap;             /* Permite que las imágenes se ajusten en varias filas si es necesario */
  }

  .referencias {
    width: 285px;                /* Ancho ajustado para las imágenes */
    height: 285px;               /* Altura ajustada para las imágenes */
    object-fit: contain;         /* Ajusta la imagen dentro del área sin recortarla */
  }

</style>
