<template>
      <div id="mapInputContainer">
        <div id="mapContainer"> </div>
        <div id="inputs">
          <h1>Datos de búsqueda</h1>
          <p>Encuentra tu carrera de montaña ideal.<br> Por ahora sólo en España y Andorra, aunque iremos incorporando más païses</p>
          <br>
          <h2>Acota las fechas</h2>
          <label for="FechaMin">Fecha Mínima: </label>
          <input id="FechaMin" type="date" v-model="dataMin" @change="this.setupMarkers()">
          <br>
          <label for="FechaMax">Fecha Màxima: </label>
          <input  id="FechaMax" type="date" v-model="dataMax" @change="this.setupMarkers()">
          <br><br>
          <h2>Acota la distancia màxima</h2>
          <label for="distanciaMax">Distancia entre 1 y 300 Km:</label>
          <br>
          <input id="distanciaMax" type="range" min="1" max="300" step="1" v-model="distanciaMax" @change="this.setupMarkers()"> 
          <br>
          ({{this.distanciaMax}} Kms)
        </div>
      </div>
</template>

<script>
  import "leaflet/dist/leaflet.css";
  import L from "leaflet";
  
  function stringToDate(str) {
    const [y, m, d] = str.split('-')
    return new Date(+y, m - 1, +d)
  }

  function dateToString(date) {
    return (
      date.getFullYear() +
      '-' +
      pad(date.getMonth() + 1) +
      '-' +
      pad(date.getDate())
    )
  }

  function pad(n, s = String(n)) {
    return s.length < 2 ? `0${s}` : s
  }

  export default{
    name:"Map",
    data(){
      return{

  curses2022:
	[
    {"nom":"12a Corre pel Cor de la Fageda","fecha":"2022-05-14","coordenades":[42.145833,2.572222],"distancies":"Sí: 15 y 23 kms","distancia":15,"web":"https://cursapelcordelafageda.cat/","pais":"ESP","valoració":0},
{"nom":"12a Corre pel Cor de la Fageda","fecha":"2022-05-14","coordenades":[42.145833,2.572222],"distancies":"Sí: 15 y 23 kms","distancia":23,"web":"https://cursapelcordelafageda.cat/","pais":"ESP","valoració":0},    
{"nom":"Mitxarro Bira Mendi Lasterketa","fecha":"2022-05-14","coordenades":[42.8908021,-2.315428],"distancies":"Sí: 19.5 y 27.5 kms","distancia":19.5,"web":"https://inscripcion.kirolprobak.com/inscripcion/mitxarro-bira-2022-carrera/","pais":"ESP","valoració":0},
{"nom":"Mitxarro Bira Mendi Lasterketa","fecha":"2022-05-14","coordenades":[42.8908021,-2.315428],"distancies":"Sí: 19.5 y 27.5 kms","distancia":27.5,"web":"https://inscripcion.kirolprobak.com/inscripcion/mitxarro-bira-2022-carrera/","pais":"ESP","valoració":0},    
{"nom":"Xtreme Lagos","fecha":"2022-05-14","coordenades":[43.3485,-5.126967],"distancies":"Sí: 42 y 15 kms","distancia":[42],"web":"https://xtremelagos.com/","pais":"ESP","valoració":0},
{"nom":"Xtreme Lagos","fecha":"2022-05-14","coordenades":[43.3485,-5.126967],"distancies":"Sí: 42 y 15 kms","distancia":[15],"web":"https://xtremelagos.com/","pais":"ESP","valoració":0},    
{"nom":"Pujada a la Font Freda","fecha":"2022-05-14","coordenades":[38.83666,-0.4960669],"distancies":"No ofrece otras distancias","distancia":[11.30],"web":"http://www.atzenetadalbaida.com/","pais":"ESP","valoració":3},
{"nom":"Cursa de la Roca Negra","fecha":"2022-05-28","coordenades":[41.34739,2.043103],"distancies":"No ofrece otras distancias","distancia":[21],"web":"http://rocanegra.cat/","pais":"ESP","valoració":5},
{"nom":"UT Muntanyes de la Costa Daurada","fecha":"2022-05-14","coordenades":[41.30876,0.9884451],"distancies":"Sí: 65,87 y 42 kms","distancia":[65],"web":"https://www.naturetime.es/ultra-trail-muntanyes-costa-daurada/","pais":"ESP","valoració":0},
{"nom":"UT Muntanyes de la Costa Daurada","fecha":"2022-05-14","coordenades":[41.30876,0.9884451],"distancies":"Sí: 65,87 y 42 kms","distancia":[87],"web":"https://www.naturetime.es/ultra-trail-muntanyes-costa-daurada/","pais":"ESP","valoració":0},
{"nom":"UT Muntanyes de la Costa Daurada","fecha":"2022-05-14","coordenades":[41.30876,0.9884451],"distancies":"Sí: 65,87 y 42 kms","distancia":[42],"web":"https://www.naturetime.es/ultra-trail-muntanyes-costa-daurada/","pais":"ESP","valoració":0},
{"nom":"Allande Extremo-SpeedTrail","fecha":"2022-05-14","coordenades":[43.26997,-6.608351],"distancies":"Sí: 17, 34 y 60 kms","distancia":[17],"web":"https://www.nortea.es/speedtrail/","pais":"ESP","valoració":3},
{"nom":"Allande Extremo-SpeedTrail","fecha":"2022-05-14","coordenades":[43.26997,-6.608351],"distancies":"Sí: 17, 34 y 60 kms","distancia":[34],"web":"https://www.nortea.es/speedtrail/","pais":"ESP","valoració":3},
{"nom":"Allande Extremo-SpeedTrail","fecha":"2022-05-14","coordenades":[43.26997,-6.608351],"distancies":"Sí: 17, 34 y 60 kms","distancia":[60],"web":"https://www.nortea.es/speedtrail/","pais":"ESP","valoració":3},
{"nom":"Trail Muntanyes Costa Daurada","fecha":"2022-05-15","coordenades":[41.30876,0.9884451],"distancies":"Sí: 22, 11 y 5 kms","distancia":[22],"web":"https://www.naturetime.es/ultra-trail-muntanyes-costa-daurada","pais":"ESP","valoració":5},
{"nom":"Trail Muntanyes Costa Daurada","fecha":"2022-05-15","coordenades":[41.30876,0.9884451],"distancies":"Sí: 22, 11 y 5 kms","distancia":[11],"web":"https://www.naturetime.es/ultra-trail-muntanyes-costa-daurada","pais":"ESP","valoració":5},
{"nom":"Trail Muntanyes Costa Daurada","fecha":"2022-05-15","coordenades":[41.30876,0.9884451],"distancies":"Sí: 22, 1 y 5 kms","distancia":[5],"web":"https://www.naturetime.es/ultra-trail-muntanyes-costa-daurada","pais":"ESP","valoració":5},
{"nom":"Castro Trail","fecha":"2022-05-15","coordenades":[42.2987305,-7.8873295],"distancies":"Sí: 12 y 24 kms","distancia":[12],"web":"https://sportmaniacs.com/es/services/inscription/castro-trail-xtreme-2022-barbadas","pais":"ESP","valoració":5},
{"nom":"Castro Trail","fecha":"2022-05-15","coordenades":[42.2987305,-7.8873295],"distancies":"Sí: 12 y 24 kms","distancia":[24],"web":"https://sportmaniacs.com/es/services/inscription/castro-trail-xtreme-2022-barbadas","pais":"ESP","valoració":5}, 
{"nom":"Cursa per Collserola","fecha":"2022-05-15","coordenades":[41.48598288,2.143696547],"distancies":"Sí: 14 y 7 kms","distancia":[14],"web":"https://www.atcerdanyola.com/curses/cursaPerCollserola/","pais":"ESP","valoració":5},
{"nom":"Cursa per Collserola","fecha":"2022-05-15","coordenades":[41.48598288,2.143696547],"distancies":"Sí: 14 y 7 kms","distancia":[7],"web":"https://www.atcerdanyola.com/curses/cursaPerCollserola/","pais":"ESP","valoració":5},
{"nom":"Trail Valle de Lena","fecha":"2022-05-17","coordenades":[43.159279,-5.829606],"distancies":"No ofrece otras distancias","distancia":[20],"web":"https://inscripciones.empa-t.com/inscripcion/ii-trail-valle-de-lena-2022/","pais":"ESP","valoració":4},
{"nom":"Trail Menorca Costa Nord","fecha":"2022-05-20","coordenades":[39.9991163,3.8389047],"distancies":"Sí: 185, 100, 85, 58, 45 y 27 kms","distancia":[185],"web":"https://trailmenorca.es/","pais":"ESP","valoració":5},
{"nom":"Trail Menorca Costa Nord","fecha":"2022-05-20","coordenades":[39.9991163,3.8389047],"distancies":"Sí: 185, 100, 85, 58, 45 y 27 kms","distancia":[100],"web":"https://trailmenorca.es/","pais":"ESP","valoració":5},
{"nom":"Trail Menorca Costa Nord","fecha":"2022-05-20","coordenades":[39.9991163,3.8389047],"distancies":"Sí: 185, 100, 85, 58, 45 y 27 kms","distancia":[85],"web":"https://trailmenorca.es/","pais":"ESP","valoració":5},
{"nom":"Trail Menorca Costa Nord","fecha":"2022-05-20","coordenades":[39.9991163,3.8389047],"distancies":"Sí: 185, 100, 85, 58, 45 y 27 kms","distancia":[58],"web":"https://trailmenorca.es/","pais":"ESP","valoració":5},
{"nom":"Trail Menorca Costa Nord","fecha":"2022-05-20","coordenades":[39.9991163,3.8389047],"distancies":"Sí: 185, 100, 85, 58, 45 y 27 kms","distancia":[45],"web":"https://trailmenorca.es/","pais":"ESP","valoració":5},
{"nom":"Trail Menorca Costa Nord","fecha":"2022-05-20","coordenades":[39.9991163,3.8389047],"distancies":"Sí: 185, 100, 85, 58, 45 y 27 kms","distancia":[27],"web":"https://trailmenorca.es/","pais":"ESP","valoració":5},
{"nom":"EDP DesafiOSOmiedo","fecha":"2022-05-21","coordenades":[43.0926945,-6.255945800000063],"distancies":"Sí: 50 y 86 kms","distancia":[50],"web":"https://www.desafiosomiedo.com/","pais":"ESP","valoració":0},
{"nom":"EDP DesafiOSOmiedo","fecha":"2022-05-21","coordenades":[43.0926945,-6.255945800000063],"distancies":"Sí: 50 y 86 kms","distancia":[86],"web":"https://www.desafiosomiedo.com/","pais":"ESP","valoració":0},
{"nom":"Escanyabocs Trail","fecha":"2022-05-21","coordenades":[42.3575784,1.4555525],"distancies":"Sí: 15 y 20 kms","distancia":[15],"web":"http://www.escanyabocs.com/","pais":"ESP","valoració":3},
{"nom":"Escanyabocs Trail","fecha":"2022-05-21","coordenades":[42.3575784,1.4555525],"distancies":"Sí: 15 y 20 kms","distancia":[20],"web":"http://www.escanyabocs.com/","pais":"ESP","valoració":3},
{"nom":"Maestrail","fecha":"2022-05-22","coordenades":[40.6483838,-0.530213300000014],"distancies":"Sí: 13, 25 y 43 kms","distancia":[13],"web":"http://www.maestrail.com/","pais":"ESP","valoració":5},
{"nom":"Maestrail","fecha":"2022-05-22","coordenades":[40.6483838,-0.530213300000014],"distancies":"Sí: 13, 25 y 43 kms","distancia":[25],"web":"http://www.maestrail.com/","pais":"ESP","valoració":4},
{"nom":"Maestrail","fecha":"2022-05-22","coordenades":[40.6483838,-0.530213300000014],"distancies":"Sí: 13, 25 y 43 kms","distancia":[43],"web":"http://www.maestrail.com/","pais":"ESP","valoració":5},
{"nom":"Trail Minero","fecha":"2022-05-22","coordenades":[40.320778,-5.633766],"distancies":"Sí: 8.5 y 21.5 kms","distancia":[8.5],"web":"https://www.clubrunning.es/carrera/?id=11776","pais":"ESP","valoració":4},
{"nom":"Trail Minero","fecha":"2022-05-22","coordenades":[40.320778,-5.633766],"distancies":"Sí: 8.5 y 21.5 kms","distancia":[21.5],"web":"https://www.clubrunning.es/carrera/?id=11776","pais":"ESP","valoració":4},
{"nom":"Trail del Jamón","fecha":"2022-05-22","coordenades":[37.0023983,-3.2665989],"distancies":"Sí: 18 y 31 kms","distancia":[18],"web":"https://racetime.es/evento/trail-del-jamon","pais":"ESP","valoració":4},
{"nom":"Trail del Jamón","fecha":"2022-05-22","coordenades":[37.0023983,-3.2665989],"distancies":"Sí: 18 y 31 kms","distancia":[31],"web":"https://racetime.es/evento/trail-del-jamon","pais":"ESP","valoració":4},
{"nom":"Zegama-Aizkorri KV","fecha":"2022-05-27","coordenades":[42.97568,-2.290072],"distancies":"No ofrece otras distancias","distancia":[3.25],"web":"https://www.zegama-aizkorri.com/kilometro-vertical","pais":"ESP","valoració":0},
{"nom":"Donostia-Hondarribia Talaia Bidea","fecha":"2022-05-27","coordenades":[43.32074,-1.984421],"distancies":"No ofrece otras distancias","distancia":[30.8],"web":"https://www.cdfortunake.com/media/attachments/2022/03/09/reglamento_2022.pdf","pais":"ESP"},
{"nom":"Spain Backyard Ultra","fecha":"2022-05-28","coordenades":[42.376281,-7.415848],"distancies":"No ofrece otras distancias","distancia":[255],"web":"https://www.spainbackyardultra.com/","pais":"ESP"},
{"nom":"Trail Casillas - ULTRA","fecha":"2022-05-28","coordenades":[40.32369390,-4.57188963],"distancies":"Sí: 13, 25, 47 y 70 kms","distancia":[13],"web":"https://racetime.es/evento/4-trail-casillas","pais":"ESP","valoració":4},
{"nom":"Trail Casillas - ULTRA","fecha":"2022-05-28","coordenades":[40.32369390,-4.57188963],"distancies":"Sí: 13, 25, 47 y 70 kms","distancia":[25],"web":"https://racetime.es/evento/4-trail-casillas","pais":"ESP","valoració":4},
{"nom":"Trail Casillas - ULTRA","fecha":"2022-05-28","coordenades":[40.32369390,-4.57188963],"distancies":"Sí: 13, 25, 47 y 70 kms","distancia":[47],"web":"https://racetime.es/evento/4-trail-casillas","pais":"ESP","valoració":4},
{"nom":"Trail Casillas - ULTRA","fecha":"2022-05-28","coordenades":[40.32369390,-4.57188963],"distancies":"Sí: 13, 25, 47 y 70 kms","distancia":[70],"web":"https://racetime.es/evento/4-trail-casillas","pais":"ESP","valoració":4},
{"nom":"Trail l'Agulla","fecha":"2022-05-28","coordenades":[41.1759037,1.3270397],"distancies":"No ofrece otras distancias","distancia":[11.5],"web":"https://trailagullaelcatllar.webnode.es/","pais":"ESP","valoració":5},
{"nom":"Trail Sant Esteve","fecha":"2022-05-29","coordenades":[41.70319735,2.436518669],"distancies":"Sí: 13 y 27 kms","distancia":[13],"web":"https://trailse.com/","pais":"ESP","valoració":4},
{"nom":"Trail Sant Esteve","fecha":"2022-05-29","coordenades":[41.70319735,2.436518669],"distancies":"Sí: 13 y 27 kms","distancia":[27],"web":"https://trailse.com/","pais":"ESP","valoració":4},
{"nom":"Cames de Ferro","fecha":"2022-05-29","coordenades":[42.1185698,2.4574027],"distancies":"Sí: 8, 10, 12, 17, 23 y 42 kms","distancia":[8],"web":"http://www.geca.cat/camesdeferro/","pais":"ESP","valoració":4},
{"nom":"Cames de Ferro","fecha":"2022-05-29","coordenades":[42.1185698,2.4574027],"distancies":"Sí: 8, 10, 12, 17, 23 y 42 kms","distancia":[10],"web":"http://www.geca.cat/camesdeferro/","pais":"ESP","valoració":4},
{"nom":"Cames de Ferro","fecha":"2022-05-29","coordenades":[42.1185698,2.4574027],"distancies":"Sí: 8, 10, 12, 17, 23 y 42 kms","distancia":[12],"web":"http://www.geca.cat/camesdeferro/","pais":"ESP","valoració":4},
{"nom":"Cames de Ferro","fecha":"2022-05-29","coordenades":[42.1185698,2.4574027],"distancies":"Sí: 8, 10, 12, 17, 23 y 42 kms","distancia":[17],"web":"http://www.geca.cat/camesdeferro/","pais":"ESP","valoració":5},
{"nom":"Cames de Ferro","fecha":"2022-05-29","coordenades":[42.1185698,2.4574027],"distancies":"Sí: 8, 10, 12, 17, 23 y 42 kms","distancia":[23],"web":"http://www.geca.cat/camesdeferro/","pais":"ESP","valoració":5},
{"nom":"Cames de Ferro","fecha":"2022-05-29","coordenades":[42.1185698,2.4574027],"distancies":"Sí: 8, 10, 12, 17, 23 y 42 kms","distancia":[42],"web":"http://www.geca.cat/camesdeferro/","pais":"ESP","valoració":5},
{"nom":"Trail Guilleries","fecha":"2022-05-29","coordenades":[41.878358311951160430908203125,2.5116317532956600189208984375],"distancies":"Sí: 11, 16, 26 y 42 kms","distancia":[11],"web":"https://trailguilleries.cat/","pais":"ESP","valoració":5},
{"nom":"Trail Guilleries","fecha":"2022-05-29","coordenades":[41.878358311951160430908203125,2.5116317532956600189208984375],"distancies":"Sí: 11, 16, 26 y 42 kms","distancia":[16],"web":"https://trailguilleries.cat/","pais":"ESP","valoració":5},
{"nom":"Trail Guilleries","fecha":"2022-05-29","coordenades":[41.878358311951160430908203125,2.5116317532956600189208984375],"distancies":"Sí: 11, 16, 26 y 42 kms","distancia":[26],"web":"https://trailguilleries.cat/","pais":"ESP","valoració":5},
{"nom":"Trail Guilleries","fecha":"2022-05-29","coordenades":[41.878358311951160430908203125,2.5116317532956600189208984375],"distancies":"Sí: 11, 16, 26 y 42 kms","distancia":[42],"web":"https://trailguilleries.cat/","pais":"ESP","valoració":5},
{"nom":"Arroyada Trail","fecha":"2022-05-29","coordenades":[41.6126587,-4.7866615],"distancies":"Sí: 15 y 27 kms","distancia":[15],"web":"http://www.arroyadatrail.es/","pais":"ESP","valoració":5},
{"nom":"Arroyada Trail","fecha":"2022-05-29","coordenades":[41.6126587,-4.7866615],"distancies":"Sí: 5 y 27 kms","distancia":[27],"web":"http://www.arroyadatrail.es/","pais":"ESP","valoració":5},
{"nom":"Els Bastions","fecha":"2022-06-04","coordenades":[42.3057487,2.1683571],"distancies":"Sí: 71, 51 y 21 kms","distancia":[71],"web":"https://www.rockthesport.com/es/evento/bastions-22/","pais":"ESP","valoració":3},
{"nom":"Els Bastions","fecha":"2022-06-04","coordenades":[42.3057487,2.1683571],"distancies":"Sí: 71, 51 y 21 kms","distancia":[51],"web":"https://www.rockthesport.com/es/evento/bastions-22/","pais":"ESP","valoració":5},
{"nom":"Els Bastions","fecha":"2022-06-04","coordenades":[42.3057487,2.1683571],"distancies":"Sí: 71, 51 y 21 kms","distancia":[21],"web":"https://www.rockthesport.com/es/evento/bastions-22/","pais":"ESP","valoració":4},
{"nom":"Ultra Trail Bosques del Sur","fecha":"2022-06-04","coordenades":[37.91055,-3.002167],"distancies":"Sí: 99, 50, 25 y 12 kms","distancia":[99],"web":"hhttps://ultratrailbosquesdelsur.es/","pais":"ESP","valoració":5},
{"nom":"Ultra Trail Bosques del Sur","fecha":"2022-06-04","coordenades":[37.91055,-3.002167],"distancies":"Sí: 99, 50, 25 y 12 kms","distancia":[50],"web":"hhttps://ultratrailbosquesdelsur.es/","pais":"ESP","valoració":4},
{"nom":"Ultra Trail Bosques del Sur","fecha":"2022-06-04","coordenades":[37.91055,-3.002167],"distancies":"Sí: 99, 50, 25 y 12 kms","distancia":[25],"web":"hhttps://ultratrailbosquesdelsur.es/","pais":"ESP","valoració":4},
{"nom":"Ultra Trail Bosques del Sur","fecha":"2022-06-04","coordenades":[37.91055,-3.002167],"distancies":"Sí: 99, 50, 25 y 12 kms","distancia":[12],"web":"hhttps://ultratrailbosquesdelsur.es/","pais":"ESP","valoració":3},
{"nom":"Ruta Vetona Ultrafondo","fecha":"2022-06-04","coordenades":[40.3865812,-5.7649619],"distancies":"Sí: 100, 41.2 y 19.2 kms","distancia":[100],"web":"https://www.rutavetona.es/","pais":"ESP","valoració":0},
{"nom":"Ruta Vetona Ultrafondo","fecha":"2022-06-04","coordenades":[40.3865812,-5.7649619],"distancies":"Sí: 100, 41.2 y 19.2 kms","distancia":[41.2],"web":"https://www.rutavetona.es/","pais":"ESP","valoració":0},
{"nom":"Ruta Vetona Ultrafondo","fecha":"2022-06-04","coordenades":[40.3865812,-5.7649619],"distancies":"Sí: 100, 41.2 y 19.2 kms","distancia":[19.2],"web":"https://www.rutavetona.es/","pais":"ESP","valoració":0},
{"nom":"Molló Trail","fecha":"2022-06-05","coordenades":[42.3467302,2.404985300000021],"distancies":"Sí: 21 y 43 kms","distancia":[21],"web":"https://www.mollo.cat/mollotrail","pais":"ESP","valoració":3},
{"nom":"Molló Trail","fecha":"2022-06-05","coordenades":[42.3467302,2.404985300000021],"distancies":"Sí: 21 y 43 kms","distancia":[43],"web":"https://www.mollo.cat/mollotrail","pais":"ESP","valoració":4},
{"nom":"Trail Lo Bunker","fecha":"2022-06-05","coordenades":[41.8768861,1.0129973000000518],"distancies":"No ofrece otras distancias","distancia":[22.14],"web":"https://lobunkertrail.blogspot.com/","pais":"ESP","valoració":5},
{"nom":"Gran Maratón Montañas de Benasque","fecha":"2022-06-11","coordenades":[42.6042858,0.5228286],"distancies":"No ofrece otras distancias","distancia":[90],"web":"https://granmaratonbenasque.es/","pais":"ESP","valoració":5},
{"nom":"Riaño Trail Run","fecha":"2022-06-10","coordenades":[42.9756044,-5.0056656],"distancies":"Sí: 15 y 23 kms","distancia":[15],"web":"https://www.rianotrail.run/","pais":"ESP","valoració":0},
{"nom":"Riaño Trail Run","fecha":"2022-06-10","coordenades":[42.9756044,-5.0056656],"distancies":"Sí: 5 y 23 kms","distancia":[23],"web":"https://www.rianotrail.run/","pais":"ESP","valoració":0},
{"nom":"Traschinepro Trail","fecha":"2022-06-05","coordenades":[42.39817,-0.500513],"distancies":"Sí: 21 y 10 kms","distancia":[21],"web":"https://www.traschinepro.com/","pais":"ESP","valoració":0},
{"nom":"Traschinepro Trail","fecha":"2022-06-05","coordenades":[42.39817,-0.500513],"distancies":"Sí: 21 y 10 kms","distancia":[10],"web":"https://www.traschinepro.com/","pais":"ESP","valoració":0},
{"nom":"Transfronterera Cap del Rec","fecha":"2022-06-11","coordenades":[-2.417165756225586, 38.49824142456055],"distancies":"Sí: 30 y 51 kms","distancia":[30],"web":"https://transfrontereracapderec.cat/","pais":"ESP","valoració":5},
{"nom":"Transfronterera Cap del Rec","fecha":"2022-06-11","coordenades":[-2.417165756225586, 38.49824142456055],"distancies":"Sí: 30 y 51 kms","distancia":[51],"web":"https://transfrontereracapderec.cat/","pais":"ESP","valoració":5},
{"nom":"Travesera integral Picos de Europa","fecha":"2022-06-11","coordenades":[43.311984,-5.05896000000007],"distancies":"No ofrece otras distancias","distancia":[74],"web":"https://www.traveserapicos.com/","pais":"ESP","valoració":5},
{"nom":"Cross Alpino del telégrafo","fecha":"2022-06-11","coordenades":[40.74145,-4.054667],"distancies":"No ofrece otras distancias","distancia":[19],"web":"https://www.maratonalpino.com/","pais":"ESP","valoració":5},
{"nom":"Maratón Alpino Madrileño","fecha":"2022-06-12","coordenades":[40.74145,-4.054667],"distancies":"No ofrece otras distancias","distancia":[44],"web":"https://www.maratonalpino.com/","pais":"ESP","valoració":4},    
{"nom":"Marathon SkyRace Vilaverdo","fecha":"2022-06-12","coordenades":[41.3346124,1.1774773],"distancies":"No ofrece otras distancias","distancia":[45],"web":"https://www.maratonalpino.com/","pais":"ESP","valoració":4},
{"nom":"Ultra Marathon Los 10000 del Soplao","fecha":"2022-06-17","coordenades":[43.3074,-4.232392],"distancies":"Sí: 48.5.5 y 112","distancia":[112],"web":"http://www.diezmildelsoplao.com/","pais":"ESP","valoració":4,"logo":"los10000delsoplado.jpg"},
{"nom":"Marathon Los 10000 del Soplao","fecha":"2022-06-17","coordenades":[43.3074,-4.232392],"distancies":"Sí: 48.5 y 112","distancia":[48.5],"web":"http://www.diezmildelsoplao.com/","pais":"ESP","valoració":4,"logo":"los10000delsoplado.jpg"},
{"nom":"Mitja Petjada. Volta Terme Sant Joan de Moró","fecha":"2022-06-18","coordenades":[40.05979111,-0.139474869],"distancies":"Sí: 9.5 y 19.5","distancia":[9.5],"web":"https://sportmaniacs.com/es/services/inscription/xvii-volta-al-terme-sant-joan-moro---petjada/","pais":"ESP","valoració":4,"logo":"petjada.jpg"},
{"nom":"Petjada. Volta Terme Sant Joan de Moró","fecha":"2022-06-18","coordenades":[40.05979111,-0.139474869],"distancies":"Sí: 9.5 y 19.5","distancia":[19.5],"web":"https://sportmaniacs.com/es/services/inscription/xvii-volta-al-terme-sant-joan-moro---petjada/","pais":"ESP","valoració":2,"logo":"petjada.jpg"},
{"nom":"IV CxM Jabata Villa Nerva (nocturna)","fecha":"2022-06-18","coordenades":[37.6954575,-6.5502621],"distancies":"No ofrece otras distancias","distancia":[21],"web":"https://ttrs.turdetaniateam.es/cxm_jabata_villa_de_nerva/","pais":"ESP","valoració":4,"logo":"jabata.png"},
{"nom":"Woman Trail","fecha":"2022-06-18","coordenades":[39.8244203,-0.2526783000000705],"distancies":"No ofrece otras distancias","distancia":[10],"web":"https://www.facebook.com/BCStrailrunnersclub/about","pais":"ESP","valoració":4,"logo":"woman-trail.jpg"},
{"nom":"Dark Trail Cieza - corto","fecha":"2022-06-18","coordenades":[43.2230443,-4.124873387159046],"distancies":"Sí: 15 y 23","distancia":[15],"web":"https://www.alcanzatumeta.es/ii-dark-trail-cieza-2022/","pais":"ESP","valoració":3,"logo":"dark-trail-cieza.png"},
{"nom":"Dark Trail Cieza","fecha":"2022-06-18","coordenades":[43.2230443,-4.124873387159046],"distancies":"Sí: 15 y 23","distancia":[23],"web":"https://www.alcanzatumeta.es/ii-dark-trail-cieza-2022/","pais":"ESP","valoració":3,"logo":"dark-trail-cieza.png"},
{"nom":"CxM Nocturna AG Montes de Ceuta","fecha":"2022-06-18","coordenades":[35.8893874,-5.3213455],"distancies":"No ofrece otras distancias","distancia":[20],"web":"https://cronofinisher.com/evento/v-cxm-nocturna-ag-montes-de-ceuta/","pais":"ESP","valoració":3,"logo":"montes-ceuta.jpg"},
{"nom":"Nit de Vèrtic-Acció Solidaria","fecha":"2022-06-18","coordenades":[41.67982,2.05931],"distancies":"No ofrece otras distancias","distancia":[13.7],"web":"https://www.facebook.com/nitdevertic/","pais":"ESP","valoració":4,"logo":"nit-de-vertic.jpg"},
{"nom":"Babia Sherpa Tour (dos jornadas)","fecha":"2022-06-18","coordenades":[52.1773862,18.0975343],"distancies":"No ofrece otras distancias","distancia":[24],"web":"http://www.babiasherpatour.com/","pais":"ESP","valoració":3,"logo":"badia-sherpa.jpg"},
{"nom":"Nocturna Renedo de Esgueva","fecha":"2022-06-18","coordenades":[41.65422,-4.627491],"distancies":"No ofrece otras distancias","distancia":[15],"web":"https://inscripciones.runvasport.es/inscripcion/vii-nocturna-renedo-de-esgueva/","pais":"ESP","valoració":2,"logo":"renedo-esgueva.png"},
{"nom":"Trail Mancha Real - Sierra Mágica - corta","fecha":"2022-06-18","coordenades":[37.7868126,-3.6103832],"distancies":"Sí: 17.7 y 23.3","distancia":[17.7],"web":"https://cronofinisher.com/evento/viii-cxm-mancha-real-sierra-magina-2022/","pais":"ESP","valoració":2,"logo":"mancha-real.png"},
{"nom":"Trail Mancha Real - Sierra Mágica - larga","fecha":"2022-06-18","coordenades":[37.7868126,-3.6103832],"distancies":"Sí: 17.7 y 23.3","distancia":[23.3],"web":"https://cronofinisher.com/evento/viii-cxm-mancha-real-sierra-magina-2022/","pais":"ESP","valoració":2,"logo":"mancha-real.png"},
{"nom":"Carrera de montaña Chandrexa 42k","fecha":"2022-06-19","coordenades":[42.42691464444984, -7.761339035440144],"distancies":"Sí: 7.3, 18, 28, y 42","distancia":[7.3],"web":"https://carrerachandrexatrail.es/","pais":"ESP","valoració":2,"logo":"chandrexa.png"},
{"nom":"Carrera de montaña Chandrexa 42k","fecha":"2022-06-19","coordenades":[42.42691464444984, -7.761339035440144],"distancies":"Sí: 7.3, 18, 28, y 42","distancia":[18],"web":"https://carrerachandrexatrail.es/","pais":"ESP","valoració":2,"logo":"chandrexa.png"},
{"nom":"Carrera de montaña Chandrexa 42k","fecha":"2022-06-19","coordenades":[42.42691464444984, -7.761339035440144],"distancies":"Sí: 7.3, 18, 28, y 42","distancia":[28],"web":"https://carrerachandrexatrail.es/","pais":"ESP","valoració":2,"logo":"chandrexa.png"},
{"nom":"Carrera de montaña Chandrexa 42k","fecha":"2022-06-19","coordenades":[42.42691464444984, -7.761339035440144],"distancies":"Sí: 7.3, 18, 28, y 42","distancia":[42],"web":"https://carrerachandrexatrail.es/","pais":"ESP","valoració":2,"logo":"chandrexa.png"},
{"nom":"Duextrem amposta trail - TRAIL","fecha":"2022-06-19","coordenades":[40.71231,0.579972],"distancies":"No ofrece otras distancias","distancia":[21],"web":"https://www.montbike.org/duextremamposta/","pais":"ESP","valoració":3,"logo":"duextrem-amposta.png"},
{"nom":"La Ribalera Marató","fecha":"2022-06-19","coordenades":[41.67982,2.05931],"distancies":"Sí: 14, 23.3 y 42 kms","distancia":[42],"web":"https://laribalera.cat/es/","pais":"ESP","valoració":3,"logo":"la-ribalera.jpg"},
{"nom":"La Ribalera Mitja Marató","fecha":"2022-06-19","coordenades":[41.67982,2.05931],"distancies":"Sí: 14, 23.3 y 42 kms","distancia":[23.3],"web":"https://laribalera.cat/es/","pais":"ESP","valoració":3,"logo":"la-ribalera.jpg"},
{"nom":"La Ribalera Cursa Popular","fecha":"2022-06-19","coordenades":[41.67982,2.05931],"distancies":"Sí: 14, 23.3 y 42 kms","distancia":[23.3],"web":"https://laribalera.cat/es/","pais":"ESP","valoració":3,"logo":"la-ribalera.jpg"},
{"nom":"Pujada a la Salut","fecha":"2022-06-19","coordenades":[42.07529738,2.50693202],"distancies":"No ofrece otras distancias","distancia":[3],"web":"https://catpescallunes.wixsite.com/pescallunes/pujada-a-la-salut","pais":"ESP","valoració":2,"logo":"pujada-a-la-salut.png"},
{"nom":"Cursa Trail El Vendrell","fecha":"2022-06-19","coordenades":[41.2196699,1.5343070000000125],"distancies":"No ofrece otras distancias","distancia":[13.4],"web":"https://trailelvendrell.runnerselvendrell.cat/","pais":"ESP","valoració":2,"logo":"trail-el-vendrell.png"},
{"nom":"Camille Extrema","fecha":"2022-06-19","coordenades":[42.86159,-0.9234196],"distancies":"Sí: 13 y 31.6 kms","distancia":[31.6],"web":"https://adi-ike.com/camille-extreme-2019/","pais":"ESP","valoració":4,"logo":"camille-extrem.png"},
{"nom":"Camille Trail","fecha":"2022-06-19","coordenades":[42.86159,-0.9234196],"distancies":"Sí: 13 y 31.6 kms","distancia":[13],"web":"https://adi-ike.com/camille-extreme-2019/","pais":"ESP","valoració":4,"logo":"camille-extrem.png"},
{"nom":"Cannelle Trail","fecha":"2022-06-19","coordenades":[42.860680, -0.924153],"distancies":"No ofrece otras distancias","distancia":[13],"web":"https://www.rockthesport.com/es/evento/canelle-trail-2018/","pais":"ESP","valoració":2,"logo":"canelle-trail.jpg"},
{"nom":"Trail Ulldeter","fecha":"2022-06-19","coordenades":[42.3136693999999,2.365142500000047],"distancies":"No ofrece otras distancias","distancia":[24],"web":"https://www.klassmark.com/trailulldeter/","pais":"ESP","valoració":3,"logo":"ull-de-ter.png"},
{"nom":"Kosta Trail - Media Maraton 21k","fecha":"2022-06-19","coordenades":[43.37892,-2.983002],"distancies":"No ofrece otras distancias","distancia":[21],"web":"https://kostatrail.com/","pais":"ESP","valoració":2,"logo":"kostra-trail.png"},
{"nom":"Lles Xtrail Short - 15k","fecha":"2022-06-19","coordenades":[42.4312362999999,1.668363799999952],"distancies":"Sí: 15, 30 y 42 Kms","distancia":[15],"web":"http://www.xtrailcup.com/","pais":"ESP","valoració":2,"logo":"xtrail-marathon-cup.png"},
{"nom":"Lles Xtrail - 30k","fecha":"2022-06-19","coordenades":[42.4312362999999,1.668363799999952],"distancies":"Sí: 15, 30 y 42 Kms","distancia":[30],"web":"http://www.xtrailcup.com/","pais":"ESP","valoració":2,"logo":"xtrail-marathon-cup.png"},
{"nom":"Lles Xtrail Large - 42k","fecha":"2022-06-19","coordenades":[42.4312362999999,1.668363799999952],"distancies":"Sí: 15, 30 y 42 Kms","distancia":[42],"web":"http://www.xtrailcup.com/","pais":"ESP","valoració":2,"logo":"xtrail-marathon-cup.png"},
{"nom":"Sprint Vertical Night 3k Beret Montgarri","fecha":"2022-06-24","coordenades":[42.73840537554788, 0.9484829706725916],"distancies":"Sí: 3, 6, y 124 kms","distancia":[3],"web":"https://beretmontgarri.com/home-summer/","pais":"ESP","valoració":2,"logo":"la-ribalera.jpg"},
{"nom":"Summer 6k Beret Montgarri","fecha":"2022-06-25","coordenades":[42.73840537554788, 0.9484829706725916],"distancies":"Sí: 3, 6, y 124 kms","distancia":[6],"web":"https://beretmontgarri.com/home-summer/","pais":"ESP","valoració":2,"logo":"la-ribalera.jpg"},
{"nom":"Summer 12k Beret Montgarri","fecha":"2022-06-25","coordenades":[42.73840537554788, 0.9484829706725916],"distancies":"Sí: 3, 6, y 124 kms","distancia":[12],"web":"https://beretmontgarri.com/home-summer/","pais":"ESP","valoració":2,"logo":"la-ribalera.jpg"},
{"nom":"Zaldibiako Mendi Lasterketa - 27km","fecha":"2022-06-25","coordenades":[43.0382572,-2.1509538],"distancies":"Sí: 15 y 28 kms","distancia":[15],"web":"https://www.rockthesport.com/es/event/zaldibiakomendilasterketa2022","pais":"ESP","valoració":4,"logo":"zaldibiako-mendi-lasterketa.jpg"},
{"nom":"Zaldibiako Mendi Lasterketa - 27km","fecha":"2022-06-25","coordenades":[43.0382572,-2.1509538],"distancies":"Sí: 15 y 28 kms","distancia":[28],"web":"https://www.rockthesport.com/es/event/zaldibiakomendilasterketa2022","pais":"ESP","valoració":4,"logo":"zaldibiako-mendi-lasterketa.jpg"},
{"nom":"Socovos Ibero Trail 21k","fecha":"2022-06-26","coordenades":[38.3218396,-1.9294681],"distancies":"Sí: 11 y 21.8 kms","distancia":[11],"web":"https://www.turismosierradelsegura.es/es/3-Eventos/106-IV-Socovos-Iberos-Trail.htm","pais":"ESP","valoració":2,"logo":"socovos-iberos-trail.png"},
{"nom":"Socovos Ibero Trail 21k","fecha":"2022-06-26","coordenades":[38.3218396,-1.9294681],"distancies":"Sí: 11 y 21.8 kms","distancia":[21.8],"web":"https://www.turismosierradelsegura.es/es/3-Eventos/106-IV-Socovos-Iberos-Trail.htm","pais":"ESP","valoració":2,"logo":"socovos-iberos-trail.png"},
{"nom":"Mini Trail de Jérez del Marquesado Rin Ran Mountain","fecha":"2022-06-19","coordenades":[37.1837778,-3.1596338],"distancies":"Sí: 12 y 25 kms","distancia":[12],"web":"https://andaluciarunning.com/carreras/trail-jerez-del-marquesado/","pais":"ESP","valoració":2,"logo":"rin-ran-mountain.jpg"},
{"nom":"Trail de Jérez del Marquesado Rin Ran Mountain","fecha":"2022-06-19","coordenades":[37.1837778,-3.1596338],"distancies":"Sí: 12 y 25 kms","distancia":[25],"web":"https://andaluciarunning.com/carreras/trail-jerez-del-marquesado/","pais":"ESP","valoració":2,"logo":"rin-ran-mountain.jpg"},
{"nom":"Subida al San Millán SSM 10K","fecha":"2022-06-26","coordenades":[42.32885448,-3.210690022],"distancies":"Sí: 10, 18 y 29 kms","distancia":[10],"web":"https://subidasanmillan.es/ssm.html","pais":"ESP","valoració":3,"logo":"subida-san-millan.png"},
{"nom":"Subida al San Millán SSM 18K","fecha":"2022-06-26","coordenades":[42.32885448,-3.210690022],"distancies":"Sí: 10, 18 y 29 kms","distancia":[10],"web":"https://subidasanmillan.es/ssm.html","pais":"ESP","valoració":3,"logo":"subida-san-millan.png"},
{"nom":"Subida al San Millán SSM 29K","fecha":"2022-06-26","coordenades":[42.32885448,-3.210690022],"distancies":"Sí: 10, 18 y 29 kms","distancia":[10],"web":"https://subidasanmillan.es/ssm.html","pais":"ESP","valoració":3,"logo":"subida-san-millan.png"},
{"nom":"Carrera por montaña Sierra de Luesia - corta","fecha":"2022-06-26","coordenades":[42.36987,-1.021661],"distancies":"Sí: 14 y 27 kms","distancia":[14],"web":"https://carrerasierradeluesia.blogspot.com/","pais":"ESP","valoració":2,"logo":"sierra-de-luesia.png"},
{"nom":"Carrera por montaña Sierra de Luesia - larga","fecha":"2022-06-26","coordenades":[42.36987,-1.021661],"distancies":"Sí: 14 y 27 kms","distancia":[27],"web":"https://carrerasierradeluesia.blogspot.com/","pais":"ESP","valoració":2,"logo":"sierra-de-luesia.png"},
{"nom":"Vallmar 35k","fecha":"2022-06-26","coordenades":[41.528475157594585, 2.4353631837203116],"distancies":"Sí: 21 y 35 kms","distancia":[35],"web":"https://montsignus.com/es/vallmar-2/","pais":"ESP","valoració":2,"logo":"vallmar.png"},
{"nom":"Crono Cas Secretari","fecha":"2022-06-26","coordenades":[39.70679158808604, 2.791891041883186],"distancies":"No ofrece otras distancias","distancia":[6],"web":"http://www.samilanaalaro.com/crono-cas-secretari.html","pais":"ESP","valoració":4,"logo":"sa-milana-peu.png"},
{"nom":"Vallhonesta x-Trail","fecha":"2022-07-01","coordenades":[41.66140278,1.863427162],"distancies":"No ofrece otras distancias","distancia":[10],"web":"https://ceaccastellet.wordpress.com/2022/01/09/x-trail-vallhonesta-corre-o-camina-sota-la-llum-de-la-lluna-divendres-01-07-22/","pais":"ESP","valoració":2,"logo":"vallhonesta.png"},
{"nom":"Garmin Epic Trail by BUFF 12K","fecha":"2022-07-02","coordenades":[42.50524050219873, 0.8009472247041894],"distancies":"Sí: 12, 23, 42 y 55 kms","distancia":[12],"web":"https://www.garminmountainfestival.com/carreras/","pais":"ESP","valoració":4,"logo":"garmin-epic_trail.png"},
{"nom":"Garmin Epic Trail by BUFF 23K","fecha":"2022-07-02","coordenades":[42.50524050219873, 0.8009472247041894],"distancies":"Sí: 12, 23, 42 y 55 kms","distancia":[23],"web":"https://www.garminmountainfestival.com/carreras/","pais":"ESP","valoració":4,"logo":"garmin-epic_trail.png"},
{"nom":"Garmin Epic Trail by BUFF 42K","fecha":"2022-07-02","coordenades":[42.50524050219873, 0.8009472247041894],"distancies":"Sí: 12, 23, 42 y 55 kms","distancia":[42],"web":"https://www.garminmountainfestival.com/carreras/","pais":"ESP","valoració":4,"logo":"garmin-epic_trail.png"},
{"nom":"Garmin Epic Trail by BUFF 55K","fecha":"2022-07-02","coordenades":[42.50524050219873, 0.8009472247041894],"distancies":"Sí: 12, 23, 42 y 55 kms","distancia":[55],"web":"https://www.garminmountainfestival.com/carreras/","pais":"ESP","valoració":4,"logo":"garmin-epic_trail.png"},
{"nom":"Puja-i-baixa a Batet","fecha":"2022-07-02","coordenades":[42.181278671277255, 2.516232891420561],"distancies":"No ofrece otras distancias","distancia":[12],"web":"https://www.facebook.com/pujaibaixaabatet/","pais":"ESP","valoració":2,"logo":"https://www.facebook.com/pujaibaixaabatet/"},                  
{"nom":"Trail de Bronchales 43k","fecha":"2022-07-02","coordenades":[40.5088378,-1.5880904],"distancies":"Sí: 13, 25 y 43 kms","distancia":[43],"web":"http://www.trailbronchales.com/","pais":"ESP","valoració":4,"logo":"trail-brinchales.png"},
{"nom":"Trail de Bronchales 25k","fecha":"2022-07-02","coordenades":[40.5088378,-1.5880904],"distancies":"Sí: 13, 25 y 43 kms","distancia":[25],"web":"http://www.trailbronchales.com/","pais":"ESP","valoració":4,"logo":"trail-brinchales.png"},
{"nom":"Trail de Bronchales 13k","fecha":"2022-07-03","coordenades":[40.5088378,-1.5880904],"distancies":"Sí: 13, 25 y 43 kms","distancia":[13],"web":"http://www.trailbronchales.com/","pais":"ESP","valoració":4,"logo":"trail-brinchales.png"},
{"nom":"Trail Fonts del Montseny - 9k","fecha":"2022-07-02","coordenades":[41.84777649999999,2.390409599999998],"distancies":"Sí: 9, 19 y 29 kms","distancia":[9],"web":"http://www.trailbronchales.com/","pais":"ESP","valoració":4,"logo":"trail-fonts-montseny.svg"},
{"nom":"Trail Fonts del Montseny - 19k","fecha":"2022-07-02","coordenades":[41.84777649999999,2.390409599999998],"distancies":"Sí: 9, 19 y 29 kms","distancia":[19],"web":"http://www.trailbronchales.com/","pais":"ESP","valoració":4,"logo":"trail-fonts-montseny.svg"},
{"nom":"Trail Fonts del Montseny - 29k","fecha":"2022-07-02","coordenades":[41.84777649999999,2.390409599999998],"distancies":"Sí: 9, 19 y 29 kms","distancia":[29],"web":"http://www.trailbronchales.com/","pais":"ESP","valoració":4,"logo":"trail-fonts-montseny.svg"},
{"nom":"Night Trail Lloret 7K","fecha":"2022-07-02","coordenades":[41.69848721,2.847776413],"distancies":"Sí: 7, 16 y 21 kms","distancia":[7],"web":"https://nighttraillloret.com/","pais":"ESP","valoració":3,"logo":"night-trail-lloret.png"},
{"nom":"Night Trail Lloret 16K","fecha":"2022-07-02","coordenades":[41.69848721,2.847776413],"distancies":"Sí: 7, 16 y 21 kms","distancia":[16],"web":"https://nighttraillloret.com/","pais":"ESP","valoració":3,"logo":"night-trail-lloret.png"},
{"nom":"Night Trail Lloret 21K","fecha":"2022-07-02","coordenades":[41.69848721,2.847776413],"distancies":"Sí: 7, 16 y 21 kms","distancia":[21],"web":"https://nighttraillloret.com/","pais":"ESP","valoració":3,"logo":"night-trail-lloret.png"},
{"nom":"Carrera por montaña Boca del Infierno - 7k","fecha":"2022-07-02","coordenades":[41.5879313,-0.6648215],"distancies":"Sí: 7, 15 y 25 kms","distancia":[7],"web":"https://bocadelinfierno.wixsite.com/bocadelinfierno","pais":"ESP","valoració":3,"logo":"boca-del-infierno.png"},
{"nom":"Carrera por montaña Boca del Infierno - 15k","fecha":"2022-07-02","coordenades":[41.5879313,-0.6648215],"distancies":"Sí: 7, 15 y 25 kms","distancia":[15],"web":"https://bocadelinfierno.wixsite.com/bocadelinfierno","pais":"ESP","valoració":3,"logo":"boca-del-infierno.png"},
{"nom":"Carrera por montaña Boca del Infierno - 25k","fecha":"2022-07-02","coordenades":[41.5879313,-0.6648215],"distancies":"Sí: 7, 15 y 25 kms","distancia":[25],"web":"https://bocadelinfierno.wixsite.com/bocadelinfierno","pais":"ESP","valoració":3,"logo":"boca-del-infierno.png"},
{"nom":"MontcalRun Marató","fecha":"2022-07-03","coordenades":[42.21932930372754,2.747199014235226],"distancies":"Sí: 8, 21 y 42 kms","distancia":[42],"web":"https://pedalmaia.cat/montcalrun/","pais":"ESP","valoració":3,"logo":"night-trail-lloret.png"},
{"nom":"MontcalRun Mitja","fecha":"2022-07-03","coordenades":[42.21932930372754,2.747199014235226],"distancies":"Sí: 8, 21 y 42 kms","distancia":[21],"web":"https://pedalmaia.cat/montcalrun/","pais":"ESP","valoració":3,"logo":"night-trail-lloret.png"},
{"nom":"MontcalRun Express","fecha":"2022-07-03","coordenades":[42.21932930372754,2.747199014235226],"distancies":"Sí: 8, 21 y 42 kms","distancia":[8],"web":"https://pedalmaia.cat/montcalrun/","pais":"ESP","valoració":3,"logo":"night-trail-lloret.png"},
{"nom":"XVI Desértica Olivera Belchite 14k","fecha":"2022-07-03","coordenades":[41.30218199737068,-0.7493562469773885],"distancies":"Sí: 26 y 14 kms","distancia":[14],"web":"www.deserticaoliverabelchite.com","pais":"ESP","valoració":3,"logo":"belchite.png"},
{"nom":"XVI Desértica Olivera Belchite 26k","fecha":"2022-07-03","coordenades":[41.30218199737068,-0.7493562469773885],"distancies":"Sí: 26 y 14 kms","distancia":[26],"web":"www.deserticaoliverabelchite.com","pais":"ESP","valoració":3,"logo":"belchite.png"},
{"nom":"La Gurrinada","fecha":"2022-07-08","coordenades":[41.5166241,2.2605775],"distancies":"No ofrece otras distancias","distancia":[11.5],"web":"https://www.facebook.com/lagurrinada/","pais":"ESP","valoració":2,"logo":"la-gurrinada.png"},
{"nom":"Corriols de Foc","fecha":"2022-07-09","coordenades":[42.1971711,2.3050256999999874],"distancies":"Sí: 20 y 30 kms","distancia":[20],"web":"https://www.corriols.cat/","pais":"ESP","valoració":4,"logo":"corriols-de-foc.png"},
{"nom":"Corriols de Foc","fecha":"2022-07-09","coordenades":[42.1971711,2.3050256999999874],"distancies":"Sí: 20 y 30 kms","distancia":[30],"web":"https://www.corriols.cat/","pais":"ESP","valoració":4,"logo":"corriols-de-foc.png"},
{"nom":"La Salvatge de Nevà","fecha":"2022-07-09","coordenades":[42.3137184,2.0783172999999806],"distancies":"No ofrece otras distancias","distancia":[15],"web":"https://www.facebook.com/LaSalvatge/","pais":"ESP","valoració":4,"logo":"la-salvatge-de-neva.jpg"},
{"nom":"Trail de Herrerías","fecha":"2022-07-09","coordenades":[43.321944,-4.496389],"distancies":"No ofrece otras distancias","distancia":[23],"web":"https://www.caminolebaniego.com/eventos/deporte/iii-trail-running-valle-de-herrerias","pais":"ESP","valoració":2,"logo":"trail-herrerias.png"},
{"nom":"Maraton La Herradura de Campoo","fecha":"2022-07-16","coordenades":[43.0153087,-4.2532041],"distancies":"Sí: 23 y 42 kms","distancia":[42.3],"web":"https://www.trailherraduradecampoo.com/","pais":"ESP","valoració":4,"logo":"la-herradura-de-campoo.png"},
{"nom":"Trail de Herrerias de Campoo","fecha":"2022-07-16","coordenades":[43.0153087,-4.2532041],"distancies":"Sí: 23 y 42 kms","distancia":[23],"web":"https://www.trailherraduradecampoo.com/","pais":"ESP","valoració":3,"logo":"la-herradura-de-campoo.png"},
{"nom":"Cursa dels Poblats Ibèrics","fecha":"2022-07-16","coordenades":[41.4812573,2.1303678],"distancies":"Sí: 10 y 16 kms","distancia":[10],"web":"https://www.facebook.com/cemcerdanyolatrail/","pais":"ESP","valoració":4,"logo":"poblats-iberics.png"},
{"nom":"Cursa dels Poblats Ibèrics","fecha":"2022-07-16","coordenades":[41.4812573,2.1303678],"distancies":"Sí: 10 y 16 kms","distancia":[16],"web":"https://www.facebook.com/cemcerdanyolatrail/","pais":"ESP","valoració":3,"logo":"poblats-iberics.png"},
{"nom":"Subida a la Mota Cetín Trail","fecha":"2022-07-16","coordenades":[43.34292,-5.15508],"distancies":"Sí: 15 y 27 kms","distancia":[15],"web":"https://www.facebook.com/motacetin/","pais":"ESP","valoració":3,"logo":"https://www.facebook.com/motacetin/"},
{"nom":"Subida a la Mota Cetín Trail","fecha":"2022-07-16","coordenades":[43.34292,-5.15508],"distancies":"Sí: 15 y 27 kms","distancia":[27],"web":"https://www.facebook.com/motacetin/","pais":"ESP","valoració":3,"logo":"https://www.facebook.com/motacetin/"},
{"nom":"Trail Muel: Cabezo de San Borombón","fecha":"2022-07-17","coordenades":[41.4686909,-1.0846228999999994],"distancies":"Sí: 10 y 20 kms","distancia":[10],"web":"https://fartlecksport.com/trail-muel/","pais":"ESP","valoració":4,"logo":"trail-muel.png"},
{"nom":"Trail Muel: Cabezo de San Borombón","fecha":"2022-07-17","coordenades":[41.4686909,-1.0846228999999994],"distancies":"Sí: 10 y 20 kms","distancia":[20],"web":"https://fartlecksport.com/trail-muel/","pais":"ESP","valoració":4,"logo":"trail-muel.png"},
{"nom":"Trail Creixell","fecha":"2022-07-23","coordenades":[41.1688664,1.4378994999999577],"distancies":"Sí: 4 y 10 kms","distancia":[4],"web":"https://creixelltrail.webnode.cat/","pais":"ESP","valoració":4,"logo":"trail-creixell.png"},
{"nom":"Trail Creixell","fecha":"2022-07-23","coordenades":[41.1688664,1.4378994999999577],"distancies":"Sí: 4 y 10 kms","distancia":[10],"web":"https://creixelltrail.webnode.cat/","pais":"ESP","valoració":4,"logo":"trail-creixell.png"},
{"nom":"Areta Trail Lasterketa","fecha":"2022-07-23","coordenades":[43.145051,-2.9434589999999616],"distancies":"No ofrece otras distancias","distancia":[13],"web":"https://sportmaniacs.com/es/services/inscription/areta-trail","pais":"ESP","valoració":4,"logo":"areta-trail-lasterketa.png"},
{"nom":"Chamizo Trail","fecha":"2022-07-23","coordenades":[36.9982006,-4.3661483],"distancies":"No ofrece otras distancias","distancia":[12],"web":"https://andaluciarunning.com/carreras/chamizo-trail-nocturno-villanueva-del-rosario/","pais":"ESP","valoració":2,"logo":"chamizo.png"},
{"nom":"Resistencia del Reino Astur","fecha":"2022-07-23","coordenades":[43.1452511,-5.701093799999967],"distancies":"No ofrece otras distancias","distancia":[43],"web":"https://www.reinoastur.es/","pais":"ESP","valoració":5,"logo":"reino-astur.png"},
{"nom":"Alkaiaga Trail","fecha":"2022-07-23","coordenades":[43.247421316399, -1.7022824325757848],"distancies":"No ofrece otras distancias","distancia":[9],"web":"http://www.manttale.com/es/carrera_alkaiaga_trail.html","pais":"ESP","valoració":4,"logo":"alkaiaga.png"},
{"nom":"Ebro Run Trail","fecha":"2022-07-23","coordenades":[42.835513,-3.7939979999999877],"distancies":"No ofrece otras distancias","distancia":[23.7],"web":"http://ebroruntrail.blogspot.com/","pais":"ESP","valoració":5,"logo":"ebro-run-trail.png"},
{"nom":"CXM Nocturna de Coripe","fecha":"2022-07-23","coordenades":[36.9726642,-5.4404374],"distancies":"No ofrece otras distancias","distancia":[24],"web":"https://inscripciones.croniussport.es/inscripcion/xvii-nocturna-de-coripe/?lang=es","pais":"ESP","valoració":3,"logo":"coripe.png"},
{"nom":"Trail Monte Dobra","fecha":"2022-07-23","coordenades":[43.32644462208241, -4.039742786704913],"distancies":"Sí: 12 y 26 kms","distancia":[12],"web":"https://www.facebook.com/Trail-Monte-Dobra-101712785755559/","pais":"ESP","valoració":3,"logo":"dobra.png"},
{"nom":"Trail Monte Dobra","fecha":"2022-07-23","coordenades":[43.32644462208241, -4.039742786704913],"distancies":"Sí: 12 y 26 kms","distancia":[26],"web":"https://www.facebook.com/Trail-Monte-Dobra-101712785755559/","pais":"ESP","valoració":3,"logo":"dobra.png"},    
{"nom":"Trail Catllaràs","fecha":"2022-07-24","coordenades":[42.2445647,1.9753087999999934],"distancies":"Sí: 15, 21 y 40 kms","distancia":[15],"web":"https://www.klassmark.com/trailcatllaras/","pais":"ESP","valoració":5,"logo":"cantallas.png"},
{"nom":"Trail Catllaràs","fecha":"2022-07-24","coordenades":[42.2445647,1.9753087999999934],"distancies":"Sí: 15, 21 y 40 kms","distancia":[21],"web":"https://www.klassmark.com/trailcatllaras/","pais":"ESP","valoració":5,"logo":"cantallas.png"},
{"nom":"Trail Catllaràs","fecha":"2022-07-24","coordenades":[42.2445647,1.9753087999999934],"distancies":"Sí: 15, 21 y 40 kms","distancia":[40],"web":"https://www.klassmark.com/trailcatllaras/","pais":"ESP","valoració":5,"logo":"cantallas.png"},
{"nom":"Trail Los Carabeos","fecha":"2022-07-31","coordenades":[42.2445647,1.9753087999999934],"distancies":"No ofrece otras distancias","distancia":[25.5],"web":"http://www.loscarabeosmtb.com/","pais":"ESP","valoració":1,"logo":"carabeos.png"},
{"nom":"Tramacastilla Trail","fecha":"2022-08-06","coordenades":[40.4306059153521,1.5741538221839118],"distancies":"Sí: 13 y 21 kms","distancia":[13],"web":"https://fartlecksport.com/tramacastilla-trail/","pais":"ESP","valoració":3,"logo":"tramacastilla.png"},
{"nom":"Tramacastilla Trail","fecha":"2022-08-06","coordenades":[40.4306059153521,1.5741538221839118],"distancies":"Sí: 13 y 21 kms","distancia":[13],"web":"https://fartlecksport.com/tramacastilla-trail/","pais":"ESP","valoració":3,"logo":"tramacastilla.png"},
{"nom":"Carrera por montaña Coto Bello","fecha":"2022-08-07","coordenades":[43.1497,-5.687873],"distancies":"No ofrece otras distancias","distancia":[12],"web":"hhttps://es-es.facebook.com/CotoBelloTrail/","pais":"ESP","valoració":2,"logo":"tcoto-bello.jpg"},
{"nom":"TVk Roca Tiraval 3,5k","fecha":"2022-08-13","coordenades":[ 42.2527708,1.8618699999999535],"distancies":"No ofrece otras distancias","distancia":[3.5],"web":"https://trailmoixero.cat/vk-roca-tiraval-2/","pais":"ESP","valoració":4,"logo":"vk-roca-tiravall.png"},
{"nom":"Cursa de l'Airosa, Memorial Felip Adell","fecha":"2022-08-13","coordenades":[40.7348171,0.3738815000000386],"distancies":"No ofrece otras distancias","distancia":[18],"web":"https://www.cursadelairosa.cat/","pais":"ESP","valoració":4,"logo":"airosa.png"},
{"nom":"Trail Moixeró","fecha":"2022-08-14","coordenades":[42.25262,1.8617110000000139],"distancies":"Sí: 12, 24 y 36 kms","distancia":[12],"web":"https://trailmoixero.cat/","pais":"ESP","valoració":4,"logo":"tmoixero-trail.png"},
{"nom":"Trail Moixeró","fecha":"2022-08-14","coordenades":[42.25262,1.8617110000000139],"distancies":"Sí: 12, 24 y 36 kms","distancia":[24],"web":"https://trailmoixero.cat/","pais":"ESP","valoració":4,"logo":"tmoixero-trail.png"},
{"nom":"Trail Moixeró","fecha":"2022-08-14","coordenades":[42.25262,1.8617110000000139],"distancies":"Sí: 12, 24 y 36 kms","distancia":[36],"web":"https://trailmoixero.cat/","pais":"ESP","valoració":4,"logo":"tmoixero-trail.png"},
{"nom":"Montlude Skyrace","fecha":"2022-08-20","coordenades":[42.811699,0.7117137],"distancies":"Sí: 10, 27 y 63 kms","distancia":[10],"web":"https://montludeskyrace.com/","pais":"ESP","valoració":3,"logo":"montlude-skyrace.png"},
{"nom":"Montlude Skyrace","fecha":"2022-08-20","coordenades":[42.811699,0.7117137],"distancies":"Sí: 10, 27 y 63 kms","distancia":[27],"web":"https://montludeskyrace.com/","pais":"ESP","valoració":3,"logo":"montlude-skyrace.png"},
{"nom":"Montlude Skyrace","fecha":"2022-08-20","coordenades":[42.811699,0.7117137],"distancies":"Sí: 10, 27 y 63 kms","distancia":[63],"web":"https://montludeskyrace.com/","pais":"ESP","valoració":3,"logo":"montlude-skyrace.png"},
{"nom":"Trail Cirat","fecha":"2022-08-20","coordenades":[40.0547973,-0.462651],"distancies":"No ofrece otras distancias","distancia":[11],"web":"https://es-la.facebook.com/TrailCirat/","pais":"ESP","valoració":2,"logo":"trail-cirat.png"},
{"nom":"CXM Mosqueruela","fecha":"2022-08-20","coordenades":[40.3609384,-0.4484679],"distancies":"Sí: 11 y 31 kms","distancia":[11],"web":"https://carrerasusscrofa.wixsite.com/misitio","pais":"ESP","valoració":2,"logo":"mosqueruela.png"},
{"nom":"CXM Mosqueruela","fecha":"2022-08-20","coordenades":[40.3609384,-0.4484679],"distancies":"Sí: 11 y 31 kms","distancia":[31],"web":"https://carrerasusscrofa.wixsite.com/misitio","pais":"ESP","valoració":2,"logo":"mosqueruela.png"},
{"nom":"Trobada Aristot TRAIL","fecha":"2022-08-21","coordenades":[42.3793131,1.6232603999999355],"distancies":"Sí: 6.2, 10, 16 y 23.5 kms","distancia":[6.2],"web":"https://montludeskyrace.com/","pais":"ESP","valoració":4,"logo":"trobada-aristot.png"},
{"nom":"Trobada Aristot TRAIL","fecha":"2022-08-21","coordenades":[42.3793131,1.6232603999999355],"distancies":"Sí: 6.2, 10, 16 y 23.5 kms","distancia":[10],"web":"https://montludeskyrace.com/","pais":"ESP","valoració":4,"logo":"trobada-aristot.png"},
{"nom":"Trobada Aristot TRAIL","fecha":"2022-08-21","coordenades":[42.3793131,1.6232603999999355],"distancies":"Sí: 6.2, 10, 16 y 23.5 kms","distancia":[16],"web":"https://montludeskyrace.com/","pais":"ESP","valoració":4,"logo":"trobada-aristot.png"},
{"nom":"Trobada Aristot TRAIL","fecha":"2022-08-21","coordenades":[42.3793131,1.6232603999999355],"distancies":"Sí: 6.2, 10, 16 y 23.5 kms","distancia":[23.5],"web":"https://montludeskyrace.com/","pais":"ESP","valoració":4,"logo":"trobada-aristot.png"},
{"nom":"Trail El Guerrero de Gredos","fecha":"2022-08-27","coordenades":[40.15509,-5.239784],"distancies":"Sí: 13, 25 y 35 kms","distancia":[13],"web":"https://trailelguerrero.com/","pais":"ESP","valoració":3,"logo":"trail-guerrero-gredos.png"},
{"nom":"Trail El Guerrero de Gredos","fecha":"2022-08-27","coordenades":[40.15509,-5.239784],"distancies":"Sí: 13, 25 y 35 kms","distancia":[25],"web":"https://trailelguerrero.com/","pais":"ESP","valoració":3,"logo":"trail-guerrero-gredos.png"},
{"nom":"Trail El Guerrero de Gredos","fecha":"2022-08-27","coordenades":[40.15509,-5.239784],"distancies":"Sí: 13, 25 y 35 kms","distancia":[35],"web":"https://trailelguerrero.com/","pais":"ESP","valoració":3,"logo":"trail-guerrero-gredos.png"},
{"nom":"La Marrana Skyrace","fecha":"2022-08-28","coordenades":[42.3751023,2.3018277],"distancies":"Sí: 11 y 23 kms","distancia":[11],"web":"https://cursalamarrana.com/","pais":"ESP","valoració":4,"logo":"marrana-skyrace.png"},
{"nom":"La Marrana Skyrace","fecha":"2022-08-28","coordenades":[42.3751023,2.3018277],"distancies":"Sí: 11 y 23 kms","distancia":[23],"web":"https://cursalamarrana.com/","pais":"ESP","valoració":4,"logo":"marrana-skyrace.png"},
{"nom":"CMC Cursa muntanya Capafonts","fecha":"2022-08-28","coordenades":[41.2954255,1.0272002000000384],"distancies":"No ofrece otras distancias","distancia":[12],"web":"http://www.cursacapafonts.com/","pais":"ESP","valoració":4,"logo":"capafonts.png"},
{"nom":"Trail 2 Heaven","fecha":"2022-09-03","coordenades":[42.60446289,0.527901649],"distancies":"Sí: 17, 26 y 50 kms","distancia":[17],"web":"http://www.trail2heaven.com/index.aspx","pais":"ESP","valoració":3,"logo":"T2H.png"},
{"nom":"Trail 2 Heaven","fecha":"2022-09-03","coordenades":[42.60446289,0.527901649],"distancies":"Sí: 17, 26 y 50 kms","distancia":[26],"web":"http://www.trail2heaven.com/index.aspx","pais":"ESP","valoració":3,"logo":"T2H.png"},
{"nom":"Trail 2 Heaven","fecha":"2022-09-03","coordenades":[42.60446289,0.527901649],"distancies":"Sí: 17, 26 y 50 kms","distancia":[50],"web":"http://www.trail2heaven.com/index.aspx","pais":"ESP","valoració":3,"logo":"T2H.png"},
{"nom":"Burriac Atac!","fecha":"2022-09-03","coordenades":[41.50306174,2.393624783],"distancies":"Sí: 15 y 21 kms","distancia":[15],"web":"http://burriacatac.cat/","pais":"ESP","valoració":4,"logo":"burriacatac.png"},
{"nom":"Burriac Atac!","fecha":"2022-09-03","coordenades":[41.50306174,2.393624783],"distancies":"Sí: 15 y 21 kms","distancia":[21],"web":"http://burriacatac.cat/","pais":"ESP","valoració":4,"logo":"burriacatac.png"},
{"nom":"Saldes Xtrail","fecha":"2022-09-03","coordenades":[42.3751023,2.3018277],"distancies":"Sí: 10, 26 y 42 kms","distancia":[10],"web":"http://www.xtrailcup.com/","pais":"ESP","valoració":3,"logo":"xtrail-marathon-cup.png"},
{"nom":"Saldes Xtrail","fecha":"2022-09-03","coordenades":[42.3751023,2.3018277],"distancies":"Sí: 10, 26 y 42 kms","distancia":[26],"web":"http://www.xtrailcup.com/","pais":"ESP","valoració":3,"logo":"xtrail-marathon-cup.png"},
{"nom":"Saldes Xtrail","fecha":"2022-09-03","coordenades":[42.3751023,2.3018277],"distancies":"Sí: 10, 26 y 42 kms","distancia":[42],"web":"http://www.xtrailcup.com/","pais":"ESP","valoració":3,"logo":"xtrail-marathon-cup.png"},
{"nom":"Saldes Xtrail","fecha":"2022-09-04","coordenades":[42.3751023,2.3018277],"distancies":"No ofrece otras distancias","distancia":[21],"web":"http://www.xtrailcup.com/","pais":"ESP","valoració":3,"logo":"xtrail-marathon-cup.png"},
{"nom":"Trail Valle de Tena","fecha":"2022-09-03","coordenades":[42.72368,-0.282123],"distancies":"Sí: 1, 2, 4 y 8 kms","distancia":[1],"web":"https://www.trailvalledetena.com/","pais":"ESP","valoració":2,"logo":"trail-valle-tena.png"},
{"nom":"Trail Valle de Tena","fecha":"2022-09-03","coordenades":[42.72368,-0.282123],"distancies":"Sí: 1, 2, 4 y 8 kms","distancia":[2],"web":"https://www.trailvalledetena.com/","pais":"ESP","valoració":2,"logo":"trail-valle-tena.png"},
{"nom":"Trail Valle de Tena","fecha":"2022-09-03","coordenades":[42.72368,-0.282123],"distancies":"Sí: 1, 2, 4 y 8 kms","distancia":[4],"web":"https://www.trailvalledetena.com/","pais":"ESP","valoració":2,"logo":"trail-valle-tena.png"},
{"nom":"Trail Valle de Tena","fecha":"2022-09-03","coordenades":[42.72368,-0.282123],"distancies":"Sí: 1, 2, 4 y 8 kms","distancia":[8],"web":"https://www.trailvalledetena.com/","pais":"ESP","valoració":2,"logo":"trail-valle-tena.png"},
{"nom":"No hay pitera","fecha":"2022-09-03","coordenades":[42.480703,-0.935576],"distancies":"Sí: 12 y 22 kms","distancia":[12],"web":"https://fartlecksport.com/no-hay-pitera/","pais":"ESP","valoració":3,"logo":"no-hay-pitera.png"},
{"nom":"No hay pitera","fecha":"2022-09-03","coordenades":[42.480703,-0.935576],"distancies":"Sí: 12 y 22 kms","distancia":[12],"web":"https://fartlecksport.com/no-hay-pitera/","pais":"ESP","valoració":3,"logo":"no-hay-pitera.png"},
{"nom":"Lezamako Mugetatik","fecha":"2022-08-28","coordenades":[-35.8728311,-57.8971069],"distancies":"No ofrece otras distancias","distancia":[25],"web":"http://www.lezamakomugetatik.com/es","pais":"ESP","valoració":3,"logo":"mugetatik.png"},
{"nom":"Pyrenees Stage Run","fecha":"2022-09-04","coordenades":[42.3057487,2.16835709999998],"distancies":"No ofrece otras distancias","distancia":[240],"web":"https://psr.run/ca/la-cursa/","pais":"ESP","valoració":4,"logo":"pyrenees-stage-run.png"},
{"nom":"Desafío Urbión","fecha":"2022-09-04","coordenades":[41.93584,-2.884975],"distancies":"No en el mismo día","distancia":[37],"web":"https://www.desafiourbion.com/","pais":"ESP","valoració":3,"logo":"desafio-urbion.png"},
{"nom":"Desafío Urbión","fecha":"2022-09-03","coordenades":[41.93584,-2.884975],"distancies":"No en el mismo día","distancia":[11],"web":"https://www.desafiourbion.com/","pais":"ESP","valoració":3,"logo":"desafio-urbion.png"},
{"nom":"Ultra Canfranc-Canfranc","fecha":"2022-09-09","coordenades":[42.71557,-0.5256487],"distancies":"No en el mismo día","distancia":[100],"web":"https://canfranccanfranc.com/","pais":"ESP","valoració":3,"logo":"canfranc-canfranc.png"},
{"nom":"Ultra Canfranc-Canfranc","fecha":"2022-09-09","coordenades":[42.71557,-0.5256487],"distancies":"No en el mismo día","distancia":[70],"web":"https://canfranccanfranc.com/","pais":"ESP","valoració":3,"logo":"canfranc-canfranc.png"},
{"nom":"Trail Fuentes Carrionas","fecha":"2022-09-18","coordenades":[42.969671,-4.757637],"distancies":"Sí: 21 y 44 kms","distancia":[21],"web":"https://utmp.run/informacion-trail/informacion-rapida-trail/","pais":"ESP","valoració":4,"logo":"UTMP.png"},
{"nom":"Trail Fuentes Carrionas","fecha":"2022-09-18","coordenades":[42.969671,-4.757637],"distancies":"Sí: 21 y 44 kms","distancia":[44],"web":"https://utmp.run/informacion-maraton/informacion-rapida-maraton-2/","pais":"ESP","valoració":4,"logo":"UTMP.png"},
{"nom":"Trail Fuentes Carrionas","fecha":"2022-09-18","coordenades":[42.969671,-4.757637],"distancies":"No en el mismo día","distancia":[7],"web":"https://utmp.run/info-vertical-trail/informacion-rapida-vertical-trail/","pais":"ESP","valoració":4,"logo":"UTMP.png"},
{"nom":"Cursa al Castell de Sant Miquel","fecha":"2022-09-11","coordenades":[41.9884916,2.8336374000000433],"distancies":"Sí: 12 y 22 kms","distancia":[12],"web":"https://santdaniel.wixsite.com/cursa-sant-miquel","pais":"ESP","valoració":3,"logo":"cursa-al-castell-de-sant-miquel.pngg"},
{"nom":"Cursa al Castell de Sant Miquel","fecha":"2022-09-11","coordenades":[41.9884916,2.8336374000000433],"distancies":"Sí: 12 y 22 kms","distancia":[22],"web":"https://santdaniel.wixsite.com/cursa-sant-miquel","pais":"ESP","valoració":3,"logo":"cursa-al-castell-de-sant-miquel.pngg"},
{"nom":"Cursa de Muntanya Vila de Falset","fecha":"2022-09-11","coordenades":[41.144826443,0.821657181],"distancies":"Sí: 13 y 26 kms","distancia":[13],"web":"http://www.cursadefalset.com/","pais":"ESP","valoració":3,"logo":"cursa-vila-de-falset.png"},
{"nom":"Cursa de Muntanya Vila de Falset","fecha":"2022-09-11","coordenades":[41.144826443,0.821657181],"distancies":"Sí: 13 y 26 kms","distancia":[26],"web":"http://www.cursadefalset.com/","pais":"ESP","valoració":3,"logo":"cursa-vila-de-falset.png"},
{"nom":"Axari Trail","fecha":"2022-09-12","coordenades":[43.01178,-2.234214],"distancies":"No ofrece otras distancias","distancia":[25.6],"web":"https://www.axaritrail.eus/es/","pais":"ESP","valoració":3,"logo":"axari.png"},
{"nom":"Ultra Trail Picos de La Demanda","fecha":"2022-09-17","coordenades":[42.3260738,-3.0138183],"distancies":"Sí: 11, 21 y 42 kms","distancia":[11],"web":"https://picosdelademanda.com/","pais":"ESP","valoració":3,"logo":"picos-de-la-demanda.png"},
{"nom":"Ultra Trail Picos de La Demanda","fecha":"2022-09-17","coordenades":[42.3260738,-3.0138183],"distancies":"Sí: 11, 21 y 42 kms","distancia":[21],"web":"https://picosdelademanda.com/","pais":"ESP","valoració":3,"logo":"picos-de-la-demanda.png"},
{"nom":"Ultra Trail Picos de La Demanda","fecha":"2022-09-17","coordenades":[42.3260738,-3.0138183],"distancies":"Sí: 11, 21 y 42 kms","distancia":[42],"web":"https://picosdelademanda.com/","pais":"ESP","valoració":3,"logo":"picos-de-la-demanda.png"},
{"nom":"Ultra Trail Picos de La Demanda","fecha":"2022-09-18","coordenades":[42.3260738,-3.0138183],"distancies":"No en el mismo día","distancia":[3],"web":"https://picosdelademanda.com/","pais":"ESP","valoració":3,"logo":"picos-de-la-demanda.png"},
{"nom":"Alameda Trail","fecha":"2022-09-17","coordenades":[40.3968493,-3.358880699999986],"distancies":"Sí: 13, 20 y 33 kms","distancia":[13],"web":"http://alamedatrailmadrid.com/","pais":"ESP","valoració":4,"logo":"alameda-trail.jpg"},
{"nom":"Alameda Trail","fecha":"2022-09-17","coordenades":[40.3968493,-3.358880699999986],"distancies":"Sí: 13, 20 y 33 kms","distancia":[13],"web":"http://alamedatrailmadrid.com/","pais":"ESP","valoració":4,"logo":"alameda-trail.jpg"},
{"nom":"Alameda Trail","fecha":"2022-09-17","coordenades":[40.3968493,-3.358880699999986],"distancies":"Sí: 13, 20 y 33 kms","distancia":[13],"web":"http://alamedatrailmadrid.com/","pais":"ESP","valoració":4,"logo":"alameda-trail.jpg"},
{"nom":"Uhartearakil-Beriain Km Bertikala","fecha":"2022-09-19","coordenades":[42.919345,-1.969194],"distancies":"No ofrece otras distancias","distancia":[5],"web":"https://uhartearakil-beriain.com/","pais":"ESP","valoració":3,"logo":"beriain.pngg"},
{"nom":"Media Maratón Valdigüelo","fecha":"2022-09-18","coordenades":[38.6156099,-6.625937],"distancies":"No ofrece otras distancias","distancia":[21],"web":"https://mediamaratonvaldiguelo.blogspot.com/","pais":"ESP","valoració":2,"logo":"desafio-urbion.png"},
{"nom":"TAGA 2040","fecha":"2022-09-18","coordenades":[42.23345063,2.285563946],"distancies":"No ofrece otras distancias","distancia":[28],"web":"https://www.taga2040.com/index.php/ca/","pais":"ESP","valoració":2,"logo":"taga2040.png"},
{"nom":"Ultra Trail Guara Somontano","fecha":"2022-09-24","coordenades":[42.17119,0.0240388],"distancies":"Sí: 30, 50 y 107 kms","distancia":[107],"web":"https://utgs.es/","pais":"ESP","valoració":4,"logo":"utgs.png"},
{"nom":"Vuelta a Sevil","fecha":"2022-09-24","coordenades":[42.17119,0.0240388],"distancies":"Sí: 30, 50 y 107 kms","distancia":[50],"web":"https://utgs.es/","pais":"ESP","valoració":3,"logo":"utgs.png"},
{"nom":"Trail del Vero","fecha":"2022-09-24","coordenades":[42.17119,0.0240388],"distancies":"Sí: 30, 50 y 107 kms","distancia":[30],"web":"https://utgs.es/","pais":"ESP","valoració":3,"logo":"utgs.png"},
{"nom":"Trail Alquézar","fecha":"2022-09-25","coordenades":[42.17119,0.0240388],"distancies":"No en el mismo días","distancia":[17],"web":"https://utgs.es/","pais":"ESP","valoració":2,"logo":"utgs.png"},
{"nom":"La Sagra Skyrace","fecha":"2022-09-24","coordenades":[37.9580561689203, -2.434003540545729],"distancies":"Sí: 16, 28, 42 y 73 kms","distancia":[16],"web":"https://www.lasagraskyrace.es/","pais":"ESP","valoració":3,"logo":"sagra.png"},
{"nom":"La Sagra Skyrace","fecha":"2022-09-24","coordenades":[37.9580561689203, -2.434003540545729],"distancies":"Sí: 16, 28, 42 y 73 kms","distancia":[28],"web":"https://www.lasagraskyrace.es/","pais":"ESP","valoració":3,"logo":"sagra.png"},
{"nom":"La Sagra Skyrace","fecha":"2022-09-24","coordenades":[37.9580561689203, -2.434003540545729],"distancies":"Sí: 16, 28, 42 y 73 kms","distancia":[42],"web":"https://www.lasagraskyrace.es/","pais":"ESP","valoració":3,"logo":"sagra.png"},
{"nom":"La Sagra Skyrace","fecha":"2022-09-24","coordenades":[37.9580561689203, -2.434003540545729],"distancies":"Sí: 16, 28, 42 y 73 kms","distancia":[73],"web":"https://www.lasagraskyrace.es/","pais":"ESP","valoració":3,"logo":"sagra.png"},

{"nom":"Travessa d'Encamp","fecha":"2022-05-27","coordenades":[42.536111,1.582778],"distancies":"Sí: 7, 12, 21, 42 y 84 kms","distancia":[7],"web":"https://otsosport.com/products/inscripcion-travessa-dencamp-andorra-2021","pais":"AND","valoració":4},
    {"nom":"Travessa d'Encamp","fecha":"2022-05-27","coordenades":[42.536111,1.582778],"distancies":"Sí: 7, 12, 21, 42 y 84 kms","distancia":[12],"web":"https://otsosport.com/products/inscripcion-travessa-dencamp-andorra-2021","pais":"AND","valoració":4},
    {"nom":"Travessa d'Encamp","fecha":"2022-05-27","coordenades":[42.536111,1.582778],"distancies":"Sí: 7, 12, 21, 42 y 84 kms","distancia":[21],"web":"https://otsosport.com/products/inscripcion-travessa-dencamp-andorra-2021","pais":"AND","valoració":4},
    {"nom":"Travessa d'Encamp","fecha":"2022-05-27","coordenades":[42.536111,1.582778],"distancies":"Sí: 7, 12, 21, 42 y 84 kms","distancia":[42],"web":"https://otsosport.com/products/inscripcion-travessa-dencamp-andorra-2021","pais":"AND","valoració":4},
    {"nom":"Travessa d'Encamp","fecha":"2022-05-27","coordenades":[42.536111,1.582778],"distancies":"Sí: 7, 12, 21, 42 y 84 kms","distancia":[84],"web":"https://otsosport.com/products/inscripcion-travessa-dencamp-andorra-2021","pais":"AND","valoració":4},
    {"nom":"Jorma Urban Trail","fecha":"2022-05-07","coordenades":[42.508889,1.540833],"distancies":"Sí: 5, 10 y 21 kms","distancia":[5],"web":"https://eventselit.com/travesses-de-muntanya/urban-trail-bike-combinada/jorma-urban-trail/","pais":"AND","valoració":3},
    {"nom":"Jorma Urban Trail","fecha":"2022-05-07","coordenades":[42.508889,1.540833],"distancies":"Sí: 5, 10 y 21 kms","distancia":[10],"web":"https://eventselit.com/travesses-de-muntanya/urban-trail-bike-combinada/jorma-urban-trail/","pais":"AND","valoració":3},
    {"nom":"Jorma Urban Trail","fecha":"2022-05-07","coordenades":[42.508889,1.540833],"distancies":"Sí: 5, 10 y 21 kms","distancia":[21],"web":"https://eventselit.com/travesses-de-muntanya/urban-trail-bike-combinada/jorma-urban-trail/","pais":"AND","valoració":3},
    {"nom":"Casamanya Extrem","fecha":"2022-06-19","coordenades":[42.555, 1.533056],"distancies":"Sí: 7, 12, 21, 42 y 84 kms","distancia":[7],"web":"https://casamanyaextrem.com/","pais":"AND","valoració":4},
    {"nom":"Casamanya Extrem","fecha":"2022-06-19","coordenades":[42.555, 1.533056],"distancies":"Sí: 7, 12, 21, 42 y 84 kms","distancia":[12],"web":"https://casamanyaextrem.com/","pais":"AND","valoració":4},
    {"nom":"Casamanya Extrem","fecha":"2022-06-19","coordenades":[42.555, 1.533056],"distancies":"Sí: 7, 12, 21, 42 y 84 kms","distancia":[21],"web":"https://casamanyaextrem.com/","pais":"AND","valoració":4},
    {"nom":"Casamanya Extrem","fecha":"2022-06-19","coordenades":[42.555, 1.533056],"distancies":"Sí: 7, 12, 21, 42 y 84 kms","distancia":[42],"web":"https://casamanyaextrem.com/","pais":"AND","valoració":4},
    {"nom":"Casamanya Extrem","fecha":"2022-06-19","coordenades":[42.555, 1.533056],"distancies":"Sí: 7, 12, 21, 42 y 84 kms","distancia":[84],"web":"https://casamanyaextrem.com/","pais":"AND","valoració":4},
    {"nom":"Trail 100 Andorra By UTMB","fecha":"2022-06-24","coordenades":[42.555, 1.533056],"distancies":"Sí: 7.5, 21, 50 y 105 kms","distancia":[7.5],"web":"https://www.trail100andorra.com/","pais":"AND","valoració":"3","logo":"trail-100-Andorra-by-UTMB.png"},
    {"nom":"Trail 100 Andorra By UTMB","fecha":"2022-06-24","coordenades":[42.555, 1.533056],"distancies":"Sí: 7.5, 21, 50 y 105 kms","distancia":[21],"web":"https://www.trail100andorra.com/","pais":"AND","valoració":"3","logo":"trail-100-Andorra-by-UTMB.png"},
    {"nom":"Trail 100 Andorra By UTMB","fecha":"2022-06-24","coordenades":[42.555, 1.533056],"distancies":"Sí: 7.5, 21, 50 y 105 kms","distancia":[50],"web":"https://www.trail100andorra.com/","pais":"AND","valoració":"3","logo":"trail-100-Andorra-by-UTMB.png"},
    {"nom":"Trail 100 Andorra By UTMB","fecha":"2022-06-24","coordenades":[42.555, 1.533056],"distancies":"Sí: 7.5, 21, 50 y 105 kms","distancia":[105],"web":"https://www.trail100andorra.com/","pais":"AND","valoració":"3","logo":"trail-100-Andorra-by-UTMB.png"},
    {"nom":"Comapedrosa","fecha":"2022-07-31","coordenades":[42.566667,1.483333],"distancies":"Sí: 50 y 23.8 kms","distancia":[50],"web":"https://skyracecomapedrosa.com/","pais":"AND","valoració":5},
    {"nom":"Comapedrosa","fecha":"2022-07-31","coordenades":[42.566667,1.483333],"distancies":"Sí: 50 y 23.8 kms","distancia":[23.8],"web":"https://skyracecomapedrosa.com/","pais":"AND","valoració":5},     
    {"nom":"Canillo Trail","fecha":"2022-08-20","coordenades":[42.566378,1.60094],"distancies":"Sí: 15 y 23,43 kms","distancia":[15],"web":"https://canillotrail.com/","pais":"AND","valoració":5},
    {"nom":"Canillo Trail","fecha":"2022-08-20","coordenades":[42.566378,1.60094],"distancies":"Sí: 15 y 23,43 kms","distancia":[23],"web":"https://canillotrail.com/","pais":"AND","valoració":5,"logo":"canillo-trail.png"},
    {"nom":"Canillo Trail","fecha":"2022-08-20","coordenades":[42.566378,1.60094],"distancies":"Sí: 15 y 23,43 kms","distancia":[43],"web":"https://canillotrail.com/","pais":"AND","valoració":5,"logo":"canillo-trail.png"},
    {"nom":"Vertical dels Isards","fecha":"2022-09-25","coordenades":[42.566378,1.60094],"distancies":"No ofrece otras distancias","distancia":[3.6],"web":"https://www.sec.ad/","pais":"AND","valoració":4},
    {"nom":"Cursa Sant Bernabé","fecha":"2022-10-02","coordenades":[42.50727417878818, 1.5499852448265685],"distancies":"No ofrece otras distancias","distancia":[11.4],"web":"https://www.cpa.ad/competicions","pais":"AND","valoració":4},
    {"nom":"VK VALLNORD CPA ","fecha":"2022-10-29","coordenades":[42.566667,1.483333],"distancies":"No ofrece otras distancias","distancia":[3.5],"web":"https://www.cpa.ad/competicions","pais":"AND","valoració":4}
],
        center:[40.436615, -2.369812],        
        map:null,
        markers:null,
        dataMin: dateToString(new Date()),
        dataMax:"2022-12-30",
        distanciaMax:50,
      }
    },

    methods:{
      setupLeafletMap(){
        this.map=L.map("mapContainer").setView(this.center,6);
        L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",{
          attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
        }).addTo(this.map);
        this.markers=L.layerGroup();
        this.map.addLayer(this.markers);

      },
      ficaTreuMarkers(cursa){
        if(cursa.fecha >= this.dataMin){
          if(cursa.fecha <= this.dataMax){
            if(new Number(cursa.distancia) <= new Number(this.distanciaMax)){
              this.markers.addLayer(L.marker([cursa.coordenades[0],cursa.coordenades[1]],{title:cursa.nom,})
                .bindPopup("<img src='./src/assets/logos/"+cursa.logo+"' width='120' style='display:block; margin-left:auto; margin-right:auto;'/><br><h2 style='text-align:center'>"+cursa.nom+"</h2><p><b>Distancia: </b>"+cursa.distancia+" Kms<br><b>Carrera con más distancias?</b><br>"+cursa.distancies+"<br><b>Valoración: </b><br><img src='./src/assets/estrellas/estrellas-"+cursa.valoració+".png' width='142' height='34' style='display:block; margin-left:auto; margin-right:auto;'/><br><a style='color:#34BE51; text-align:center; display:block; margin-left:auto; margin-right:auto;' href="+cursa.web+" target='_blank'>Web carrera</a></p>" )

              );
            }
          }
        }     
      },
      setupMarkers(){
        this.markers.clearLayers();
        this.curses2022.forEach(cursa=>this.ficaTreuMarkers(cursa));
      },

    },
    mounted() {
      this.setupLeafletMap();
      this.setupMarkers();
    },

  };
</script>
<style scoped>



  #mapInputContainer {
    display:grid;
    grid-template-columns: repeat(6,1fr);
    grid-template-rows: repeat(2,1fr);
    
    gap:20px
  }
    #mapContainer{
      grid-column:1/5 ;
      grid-row:1/3;

         
    }
    #inputs{
      grid-column:5/7 ;
    }
    a:link, a:visited{
      background-color: #34BE51;color:white; padding: 7px,10px; text-align: center; text-decoration: none; display:inline-block;
    }
    a:hover,a:active{
      background-color: #4D4D4D;
    }

div.hidden {
  display: none;
}
</style>
