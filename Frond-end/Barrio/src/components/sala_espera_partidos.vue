
<template>
  <button class="btn-volver" @click="$router.go(-1)">Volver</button>

    <div class="sala bg-black text-white min-h-screen flex flex-col items-center p-6 gap-6">
  
      <!-- Parte superior: Logo y nombre del partido -->
      <div class="caja_arriba">
        <img :src="partido.logo" alt="Logo del Partido" class="imagen" />
        <h1 class="nombree">{{ partido.nombre }}</h1> 
      </div>
      <!-- Creador del partido -->
       <div>
      <div class="creador_partido">
        <h1 class="text_cread">Creador: </h1>
        <img :src="creador.logo" alt="Logo Creador" class="creador" />
        <p class="texto_creador">{{ creador.nombre }}</p>
      </div>
    </div>
  
      <!-- Equipos y botón dentro de contenedor -->
      <div class="caja_papa">
  
        <!-- Equipos con "VS" al centro -->
        <div class="caja_vs">
          <!-- Equipo 1 -->
          <div class="flex flex-col items-center bg-gray-800 p-4 rounded-xl w-full md:w-1/3">
            <img :src="equipo1.logo" alt="Logo Equipo 1" class="equipo1" />
            <p class="letra_equipo">{{ equipo1.nombre }}</p>
          </div>
  
          <!-- VS -->
          <div class="vs-text">VS</div>
  
  
          <!-- Equipo 2 -->
          <!-- Equipo 2 -->
  <div v-if="equipo2.nombre" class="flex flex-col items-center bg-gray-800 p-4 rounded-xl w-full md:w-1/3">
    <img :src="equipo2.logo" alt="Logo Equipo 2" class="equipo1" />
    <p class="letra_equipo2">
      {{ equipo2.nombre }}
    </p>
  </div>
  
  <div v-else class="">
    <img :src="equipo2.logo || 'https://ih1.redbubble.net/image.3848000300.8770/flat,750x,075,f-pad,750x1000,f8f8f8.jpg'" 
       alt="Logo Equipo 2" 
       class="equipo10" />
    <p class="letra_equipo20">
    Esperando equipo 😖​😴​... <br>
      mira el buzon
    </p>
  </div>
  
        </div>
  
        <!-- Botón de iniciar partido -->
        <div v-if="equipo2.nombre">
          <button class="btn-gamer" @click="iniciarPartido">
    Iniciar Partido
  </button>
        </div>
      </div>
  <!-- Buzón de equipos -->
  <!-- Botón para abrir/cerrar el buzón -->
  <div v-if="equipo2.nombre">
    <button @click="mostrarModal = true" class="cancelar-btn">
      Cancelar Enfrentamiento
    </button>
  
    <!-- Modal -->
    <div v-if="mostrarModal" class="modal-overlay">
      <div class="modal-content">
        <p>⚠️ Si cancelas el enfrentamiento, tu equipo puede perder puntos.</p>
        <div class="modal-btns">
          <button @click="confirmarCancelacion" class="confirmar">Confirmar</button>
          <button @click="mostrarModal = false" class="cerrar">Cerrar</button>
        </div>
      </div>
    </div>
  </div>
  
  <div v-else class="my-4">
    <button @click="toggleBuzon" class="btn-toggle">
      {{ mostrarBuzon ? 'Cerrar Buzón 📪' : 'Abrir Buzón de Equipos 📬' }}
    </button>
  </div>
  
  <!-- Buzón de Equipos -->
  <div v-if="mostrarBuzon && buzonEquipos.length" class="buzon-container">
    <h2 class="buzon-title">📬 Buzón de Equipos que desean competir</h2>
    <p  class="buzon-title2">"Solo tienes una oportunidad. Elige con inteligencia."</p>
    <div
      v-for="(equipo, index) in buzonEquipos"
      :key="index"
      class="buzon-equipo"
    >
      <div class="equipo-info">
        <img :src="equipo.logo" alt="Logo del equipo" />
        <p class="equipo-nombre">{{ equipo.nombre }}</p>
      </div>
      <div class="buzon-btns">
        <button @click="aceptarEquipo(index)" class="aceptar">Aceptar</button>
        <button @click="rechazarEquipo(index)" class="rechazar">Rechazar</button>
      </div>
    </div>
  </div>
  
      <!-- Información del partido -->
      <div class="">
      <!-- Información del partido -->
      <div class="informacion">
        <div class="bebe">
          <h1 class="info">Informacion</h1>
          <p><span class="text-gold font-semibold">Reglas:</span> {{ partido.reglas }}</p>
          <p><span class="text-gold font-semibold">Hora:</span> {{ partido.hora }}</p>
          <p><span class="text-gold font-semibold">Día:</span> {{ partido.dia }}</p>
          <p><span class="text-gold font-semibold">Apuesta:</span> {{ partido.apuesta }}</p>
          <p><span class="text-gold font-semibold">Ubicación:</span> {{ partido.ubicacion }}</p>
          <p><span class="text-gold font-semibold">Tipo de Fútbol:</span> {{ partido.tipo }}</p>
          <p><span class="text-gold font-semibold">Cómo llegar:</span> {{ partido.comoLlegar }}</p>
          <p><span class="text-gold font-semibold">Foto cancha:</span></p>
          
          <!-- Imagen de la cancha con click para agrandar -->
          <img :src="partido.imagenCancha" alt="Imagen de la cancha" class="cancha-img" @click="ampliarImagen = true" />
  
          <a
            :href="'https://www.google.com/maps?q=' + partido.ubicacion"
            target="_blank"
            class="ubi"
          >
            Ver en Google Maps ubicación
          </a>
        </div>
      </div>
  
      <!-- Modal de la imagen -->
      <div v-if="ampliarImagen" class="overlay-img" @click="ampliarImagen = false">
    <img :src="partido.imagenCancha" alt="Cancha Ampliada" class="imagen-grande" />
  </div>
    </div>
    </div>
  </template>
  <script>
  export default {
    name: "SalaDeEspera",
    data() {
      return {
        ampliarImagen: false,
  
        mostrarModal: false,
        partido: {
          nombre: 'Clásico del Valle',
          logo: "https://marketplace.canva.com/EAGIau7JQhI/1/0/1600w/canva-logo-club-f%C3%BAtbol-ilustrado-azul-y-naranja-VJNS-j2_xd8.jpg",
          reglas: 'Gana el mejor de 3 tiempos',
          hora: '16:00',
          dia: '27/03/2024',
          apuesta: '$500.000',
          ubicacion: '4.7208726, -74.253135',
          imagenCancha: 'https://ichef.bbci.co.uk/ace/ws/640/cpsprodpb/238D/production/_95410190_gettyimages-488144002.jpg.webp',
          tipo: 'Fútbol 7',
          comoLlegar: 'Cra 12 #45-67, Barrio Centro'
        },
        equipo1: {
          nombre: 'Agua',
          logo: 'https://www.zarla.com/images/zarla-aqua-1x1-2400x2400-20220126-kpct6tpmyv8fxqq34dgf.png?crop=1:1,smart&width=250&dpr=2'
        },
        equipo2: {
          nombre: '',
          logo: ''
        },
        creador: {
          nombre: 'Carlos',
          logo: 'https://marketplace.canva.com/EAGDCjTCnYM/2/0/1600w/canva-logo-club-de-futbol-juvenil-deportivo-azul-E7n8dK6P4es.jpg'
        },
        buzonEquipos: [
          {
            nombre: 'Furia Roja',
            logo: 'https://cdn-icons-png.flaticon.com/512/616/616408.png'
          },
          {
            nombre: 'Los Panteras rowapsaaklla aslsl',
            logo: 'https://cdn-icons-png.flaticon.com/512/616/616408.png'
          }
        ],
        mostrarBuzon: false // Estado para abrir/cerrar el buzón
      };
    },
    methods: {
      aceptarEquipo(index) {
    const confirmado = confirm(`¿Estás seguro de aceptar al equipo "${this.buzonEquipos[index].nombre}"?`);
    if (confirmado) {
      this.equipo2.nombre = this.buzonEquipos[index].nombre;
      this.equipo2.logo = this.buzonEquipos[index].logo;
      this.buzonEquipos.splice(index, 1);
      this.mostrarBuzon = false; // 🔒 Cierra el buzón
      alert('Equipo aceptado ✅');
    }
  },
  iniciarPartido() {
  const hoy = new Date();
  const [dia, mes, año] = this.partido.dia.split('/').map(Number);
  const fechaPartido = new Date(año, mes - 1, dia); // JavaScript cuenta los meses desde 0

  if (hoy < fechaPartido) {
    alert("⏳ El partido aún no puede comenzar. Espera al día programado.");
  } else {
    // Cambia '/nombre-de-tu-componente' por la ruta real
    this.$router.push({ name: 'resultados_partidos' }); 
  }
},
      confirmarCancelacion() {
    const razon = prompt("¿Por qué deseas cancelar el enfrentamiento?");
    if (razon && razon.trim() !== "") {
      // Aquí puedes enviar la razón a tu backend o mostrarla en consola
      console.log("Motivo de cancelación:", razon);
      alert("Partido cancelado. El equipo puede perder puntos.");
      this.mostrarModal = false;
    } else {
      alert("Debes ingresar una razón para cancelar el enfrentamiento.");
    }
  },
      rechazarEquipo(index) {
        const confirmado = confirm(`¿Estás seguro de rechazar al equipo "${this.buzonEquipos[index].nombre}"?`);
        if (confirmado) {
          this.buzonEquipos.splice(index, 1);
          alert('Equipo rechazado ❌');
        }
      },
      toggleBuzon() {
        this.mostrarBuzon = !this.mostrarBuzon;
      }
    }
  };
  </script>
  
  <style scoped>
  .sala {
    background-color: #000;
    color: #fff;
    min-height: 100vh;
    min-width: 1200px;
    border: solid white;
    padding: 5%;
  }
  
  .text-gold {
    color: #8a8a8a;
    font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
    text-shadow: 0 0 10px white;
  }
  
  .border-gold {
    border-color: #FFD700;
  }
  
  .btn-gamer {
    background: linear-gradient(90deg, #4a4a4a, #000000);
    color:  rgb(255, 191, 0);
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 0.75rem;
    border: solid rgb(255, 191, 0);
    font-weight: bold;
    font-size: 1.8rem;
    cursor: pointer;
    transition: transform 0.2s, box-shadow 0.2s;
    margin-top: 5%;
  }
  
  .btn-gamer:hover {
    transform: scale(1.05);
    box-shadow: 0 0 12px #FFD700;
  }
  
  /* Imagen de la cancha */
  .cancha-img {
    width: 60%;
    height: 60%;
    max-height: 300px;
    object-fit: cover;
    margin-top: 1rem;
    border-radius: 1rem;
    border: 2px solid #FFD700;
    
  }
  
  /* Responsivo para logos */
  img {
    max-width: 100%;
    height: auto;
    display: block;
  }
  
  
  .equipo1 {
    width: 200px;
    height: 200px;
    border-radius: 50%;
    border: 4px solid orange;
    object-fit: cover;
    transition: transform 0.4s ease, box-shadow 0.4s ease;
    box-shadow: 0 0 20px rgb(255, 255, 255);
    cursor: pointer;
  }
  
  .equipo1:hover {
    transform: scale(1.1) rotate(2deg);
    box-shadow: 0 0 25px 5px #ff8c00;
  }
  
  .equipo10 {
    width: 200px;
    height: 200px;
    border-radius: 50%;
    border: 4px solid rgb(58, 58, 58);
    object-fit: cover;
    transition: transform 0.4s ease, box-shadow 0.4s ease;
    box-shadow: 0 0 20px rgb(255, 255, 255);
    cursor: pointer;
  }
  
  .equipo10:hover {
    transform: scale(1.1) rotate(2deg);
    box-shadow: 0 0 25px 5px #030303;
  }
  
  @keyframes fadeInLeft {
    from {
      opacity: 0;
      transform: translateX(-20px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }
  
  .creador_partido {
    display: flex;
    align-items: center;
    gap: 20px;
    background: linear-gradient(to right, #1a1a1a, #2b2b2b);
    border: 2px solid #ffffff;
    border-radius: 12px;
    padding: 12px 20px;
    margin-top: 20px;
    box-shadow: 0 0 15px rgba(255, 255, 255, 0.1);
    animation: fadeInLeft 0.8s ease-out;
    position: relative;
    overflow: hidden;
    width: 30%;
  }
  
  .creador_partido::before {
    content: '';
    position: absolute;
    top: -40%;
    left: -40%;
    width: 180%;
    height: 180%;
    background: radial-gradient(circle, rgba(255, 215, 0, 0.05), transparent 70%);
    animation: pulseGlow 4s infinite;
    z-index: 0;
  }
  
  .creador {
    height: 60px;
    width: 60px;
    border-radius: 50%;
    border: 2px solid #FFD700;
    object-fit: cover;
    box-shadow: 0 0 10px rgba(255, 215, 0, 0.4);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    z-index: 1;
  }
  
  .creador:hover {
    transform: scale(1.1);
    box-shadow: 0 0 15px rgba(255, 215, 0, 0.6);
  }
  
  .text_cread {
    font-size: 1.3rem;
    color: #FFD700;
    font-weight: bold;
    z-index: 1;
    text-shadow: 1px 1px 3px #000;
  }
  
  .texto_creador {
    font-size: 1.2rem;
    color: white;
    z-index: 1;
    font-weight: 500;
    text-shadow: 1px 1px 2px #444;
  }
  
  @keyframes fadeInScale {
    from {
      opacity: 0;
      transform: scale(0.95);
    }
    to {
      opacity: 1;
      transform: scale(1);
    }
  }
  
  .caja_vs {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    gap: 80px;
    padding: 5%;
    border: 2px solid #FFD700;
    border-radius: 20px;
    background-image: linear-gradient(
        rgba(18, 18, 18, 0.644),
        rgba(18, 18, 18, 0.516)
      ),
      url("https://www.fcbarcelona.com/photo-resources/2019/06/28/4e3d0453-cb02-45fb-88cf-e47c57c677d4/morro_da_mineira.jpg?width=1200&height=750");
    background-size: cover;
    background-position: center;
    box-shadow: 0 0 20px rgb(255, 255, 255);
    animation: fadeInScale 0.8s ease-out;
    position: relative;
    overflow: hidden;
    transition: all 0.4s ease-in-out;
  }
  
  .caja_vs::before {
    content: "";
    position: absolute;
    inset: 0;
    background: radial-gradient(circle at center, rgba(255, 215, 0, 0.05), transparent 80%);
    z-index: 0;
  }
  
  .caja_vs > * {
    z-index: 1; /* Asegura que el contenido esté por encima del overlay */
  }
  .caja_vs:hover {
    transform: scale(1.03);
    box-shadow: 0 0 30px rgba(255, 225, 1, 0.8);
    transition: all 0.4s ease-in-out;
    border-color: #ffaa00;
  }
  
  .caja_papa{
    padding: 3%;
    display: flex;
    justify-content: center;
    flex-direction:column;
    align-items: center;
    
  }
  
  .vs-text {
    font-family: 'Orbitron', sans-serif;
    font-size: 4rem;
    color: #FFD700;
    text-transform: uppercase;
    letter-spacing: 10px;
    text-shadow: 0 0 8px #FF8C00, 0 0 12px #FFA500;
    animation: pulseVs 1.5s infinite ease-in-out;
  }
    
  @keyframes pulseVs {
    0% {
      transform: scale(1);
      text-shadow: 0 0 8px #FF8C00;
    }
    50% {
      transform: scale(1.15);
      text-shadow: 0 0 12px #FFA500, 0 0 18px #FF4500;
    }
    100% {
      transform: scale(1);
      text-shadow: 0 0 8px #FF8C00;
    }
  }
  .letra_equipo {
    background: linear-gradient(90deg, #ff0000, #990000);
    color: white;
    font-size: 2rem;
    font-weight: bold;
    text-align: center;
    padding: 10px 20px;
    margin-top: 30px;
    border: 2px solid #ff4d00;
    border-radius: 12px;
    text-shadow: 2px 2px 4px #000;
    box-shadow: 0 0 15px rgba(255, 0, 0, 0.6);
    font-family: 'Orbitron', sans-serif;
    letter-spacing: 2px;
    text-transform: uppercase;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  
    /* ✅ Evita que el texto se desborde */
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    max-width: 280px;
    width: 100%;
  }
  
  
  .letra_equipo:hover {
    transform: scale(1.05);
    box-shadow: 0 0 25px rgba(255, 0, 0, 0.9);
  }
  .letra_equipo2 {
    background: linear-gradient(90deg, #28d4e7, #0eddbe);
    color: white;
    font-size: 2rem;
    font-weight: bold;
    text-align: center;
    padding: 10px 20px;
    margin-top: 30px;
    border: 2px solid #00bfff;
    border-radius: 12px;
    text-shadow: 2px 2px 4px #000;
    box-shadow: 0 0 15px rgba(2, 160, 142, 0.6);
    font-family: 'Orbitron', sans-serif;
    letter-spacing: 2px;
    text-transform: uppercase;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    max-width: 280px;
    width: 100%;
    height: 50%;
  
    /* 🧠 Estas son las claves para lo que pediste */
    overflow: hidden;         /* Oculta el texto que se sale */
    text-overflow: ellipsis;  /* Agrega "..." si se corta */
    white-space: nowrap;      /* Mantiene el texto en una sola línea */
  }
  
  .letra_equipo2:hover {
    transform: scale(1.05);
    box-shadow: 0 0 25px rgba(0, 255, 242, 0.9);
  }
  .informacion{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
   
  }
  .bebe{
    border-top: white solid;
  }
  .info{
    text-align: center;
    font-family: cursive;
    text-shadow: 0 0 10px orange;
  }
  .letra_equipo20 {
    background: linear-gradient(135deg, #4f4f4f, #2e2e2e);
    color: #fdfdfdf0;
    font-size: 1.8rem;
    font-weight: normal;
    text-align: center;
    padding: 12px 24px;
    margin-top: 30px;
    border: 1px solid #666;
    border-radius: 10px;
    text-shadow: 1px 1px 2px #111;
    box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.5);
    font-family: 'Courier New', monospace;
    letter-spacing: 1px;
    text-transform: none;
    transition: all 0.3s ease;
    max-width: 220px;
    word-wrap: break-word;
    white-space: normal;
    font-size: 100%;
    height: auto;
    opacity: 0.85;
  }
  
  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  .caja_arriba {
    background: linear-gradient(to right, #000000, #2c2c2c);
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Georgia', serif;
    padding: 1.5%;
    border: 2px solid #FFD700;
    border-radius: 16px;
    gap: 40px;
    box-shadow: 0 0 20px rgba(255, 215, 0, 0.2);
    animation: fadeInUp 0.8s ease-out;
    position: relative;
    overflow: hidden;
  }
  .buzon-container {
    background-color: #111; /* negro */
    color: white;
    padding: 20px;
    border-radius: 15px;
    max-width: 700px;
    margin: 30px auto;
    box-shadow: 0 0 10px rgba(218, 165, 32, 0.4); /* sombra dorada */
  }
  
  .buzon-title {
    color: #daa520; /* dorado */
    text-align: center;
    font-size: 24px;
    
    font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  }
  
  .buzon-title2{
    color: #cacaca; /* dorado */
    text-align: center;
    font-size: 15px;
    margin-bottom: 20px;
    font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  }
  .buzon-equipo {
    background-color: #1e1e1e; /* gris oscuro */
    border-left: 5px solid #daa520;
    margin-bottom: 15px;
    padding: 15px;
    border-radius: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .equipo-info {
    display: flex;
    align-items: center;
    gap: 15px;
  }
  
  .equipo-info img {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    border: 2px solid #daa520;
    object-fit: cover;
  }
  
  .equipo-nombre {
    font-size: 18px;
    font-weight: bold;
    color: white;
  }
  
  .buzon-btns {
    display: flex;
    gap: 10px;
  }
  
  .aceptar, .rechazar {
    padding: 8px 16px;
    border: none;
    border-radius: 6px;
    font-weight: bold;
    cursor: pointer;
    transition: 0.3s;
  }
  
  .aceptar {
    background-color: #daa520; /* dorado */
    color: black;
  }
  
  .aceptar:hover {
    background-color: #e5c100;
  }
  
  .rechazar {
    background-color: #555; /* gris */
    color: white;
  }
  
  .rechazar:hover {
    background-color: #777;
  }
  
  .btn-toggle {
    background-color: #daa520;
    color: black;
    font-weight: bold;
    padding: 10px 20px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    transition: 0.3s;
  }
  
  .btn-toggle:hover {
    background-color: #e5c100;
  }
  
  .caja_arriba::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: radial-gradient(circle, rgba(255, 255, 255, 0.1), transparent 70%);
    animation: pulseGlow 3s infinite;
    z-index: 0;
  }
  
  @keyframes pulseGlow {
    0%, 100% {
      transform: scale(1);
      opacity: 0.4;
    }
    50% {
      transform: scale(1.2);
      opacity: 0.7;
    }
  }
  
  .imagen {
    height: 70px;
    width: 70px;
    border-radius: 50%;
    border: 2px solid #FFD700;
    object-fit: cover;
    box-shadow: 0 0 12px rgba(255, 215, 0, 0.6);
    transition: transform 0.4s ease, box-shadow 0.4s ease;
    z-index: 1;
  }
  
  .imagen:hover {
    transform: scale(1.15);
    box-shadow: 0 0 20px rgba(255, 215, 0, 0.9);
  }
  
  .nombree {
    color: white;
    font-size: 2.5rem;
    font-weight: bold;
    text-shadow: 2px 2px 6px #FFD700;
    letter-spacing: 1.5px;
    z-index: 1;
  }
  .ubi {
    background-color: rgb(77, 79, 78);
    color: white;
    padding: 12px 24px;
    margin-top: 3%;
    font-family: Georgia, 'Times New Roman', Times, serif;
    font-size: 1rem;
    border: 2px solid #5e5e5e;
    border-radius: 8px;
    cursor: pointer;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    transition: all 0.3s ease;
    box-shadow: 0 4px 6px rgba(0,0,0,0.2);
    border: white solid;
  }
  
  .ubi:hover {
    background-color: #a3a3a3;
    color: black;
    transform: translateY(-2px);
    box-shadow: 0 6px 10px rgba(0,0,0,0.3);
  }
  .cancelar-btn {
    background-color: #cc0000;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 8px;
    font-weight: bold;
    cursor: pointer;
  
  }
  
  .modal-overlay {
    color: black;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.6);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
  }
  
  .modal-content {
    background: #fff;
    padding: 20px;
    border-radius: 12px;
    max-width: 400px;
    text-align: center;
    box-shadow: 0 0 15px #000;
  }
  
  .modal-content p {
    font-weight: bold;
    margin-bottom: 20px;
  }
  
  .modal-btns button {
    margin: 0 10px;
    padding: 8px 16px;
    border-radius: 6px;
    font-weight: bold;
    cursor: pointer;
  }
  
  .confirmar {
    background-color: #d9060a;
    color: white;
  }
  
  .cerrar {
    background-color: #374151;
    color: white;
  }
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.8);
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .cancha-img {
    width: 100%;
    max-width: 500px;
    border: 3px solid gold;
    border-radius: 20px;
    box-shadow: 0 0 15px rgba(255, 215, 0, 0.6);
    cursor: pointer;
    transition: transform 0.3s ease;
  }
  
  .cancha-img:hover {
    transform: scale(1.05);
  }
  
  /* Estilo del modal */
  .overlay-img {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.9);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 999;
  }
  
  .imagen-grande {
    max-width: 90%;
    max-height: 90%;
    border: 4px solid gold;
    border-radius: 20px;
    box-shadow: 0 0 25px gold;
  }
  .btn-volver {
  background-color: #111111; /* negro profundo */
  color: #ffd700; /* dorado */
  border: 2px solid #ffd700;
  padding: 12px 24px;
  border-radius: 12px;
  font-weight: bold;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 0 12px rgba(255, 215, 0, 0.2);
  text-shadow: 1px 1px 2px #000;
}

.btn-volver:hover {
  background-color: #222222; /* más gris oscuro */
  color: #ffffff;
  box-shadow: 0 0 18px rgba(255, 215, 0, 0.5);
  transform: scale(1.05);
}

  </style>
  