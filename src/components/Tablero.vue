<template>
  <div>
    <p v-if="!fin">Turno: {{turno}}</p>
    <p v-if="fin">{{ganador}}</p>
    <table>
      <tr v-for= "(fila, indexf) in casillas" :key= "indexf">
        <td v-for= "(casilla, indexc) in fila" :key= "indexc" @click= "ponerFicha(indexf,indexc,false)">{{casilla}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
const numTiles= 4;
const numPlayers= 3;
const playerSimbols = ["X","O","F"];

export default {
  name: 'Tablero',
  props: ["eventbus"],
  data: function() {
    var tablero = new Array(numTiles);
    var isAuto = true;
    var juega = 1;
    var fin = false;
    var ganador = "" ;
    for (let i = 0; i < numTiles ; i++){
      tablero[i] = new Array(numTiles);
      for(let j = 0; j< numTiles;j++){
        tablero [i][j] = "";
      }
    }
    return{
      tablero: tablero,
      isAuto: isAuto,
      juega: juega,
      fin: fin,
      ganador:ganador,
      turno: 1
    }
  },
  created(){
    this.eventbus.$on("jugar_auto",function() {this.reiniciar(true);}.bind(this));
    this.eventbus.$on("jugar_versus",function() {this.reiniciar(false);}.bind(this));

  },
  methods:{
    ponerFicha(fila,columna,auto){
      if(this.turno > 0){
        console.log("ponemos ficha");
        if (!auto){
          if(this.tablero[fila][columna] === "")  {

            /*
            //Otra forma de actualizar activando la reactividad podría ser:
            const temp = this.tablero[fila].slice(0)
            temp[columna] = playerSimbols[this.juega -1];
            this.$set(this.tablero,fila,temp);
            */
            this.tablero[fila].splice(columna,1,playerSimbols[this.juega-1]);
            this.turno ++;
            if(this.juega === numPlayers){
                this.juega = 1;
            }else{
                this.juega++;
            }
            this.comprobarVictoria();
            if(this.isAuto){
              this.ponerFicha(0,0,true);
            }
          }else{
            console.log("Hay algo");
          }
        }else{
          for(var i=1 ; i< numPlayers; i++){
            let flag = false;
            do{
              var f = Math.trunc(Math.random()*(numTiles));
              var c = Math.trunc(Math.random()*(numTiles));
              console.log("Fila: "+f+", Columna: "+c);
              if(this.tablero[f][c] === ""){
                flag = true;
              }                  
            }while(!flag);
            this.tablero[f].splice(c,1,playerSimbols[this.juega-1]);
            this.turno++;
            if(this.juega === numPlayers){
              this.juega = 1;
            }else{
              this.juega++;
            }
            this.comprobarVictoria();
            if(this.turno <0){
              return;
            }
          }
        }
      }
    },
    comprobarVictoria(){
        for(let i = 0; i<numTiles ; i++){
            let temp = this.tablero[i][0];
            let temp2 = this.tablero[0][i];
            let numIgualesF = 0;
            let numIgualesC = 0;
            let numIgualesDS = 0;
            let numIgualesDI = 0;
            for(let j = 0; j<numTiles;j++){
                if(j>0 && temp && temp.localeCompare(this.tablero[i][j]) === 0){
                    numIgualesF++;
                }
                if(i === 0 && temp && j > 0){
                    if(temp.localeCompare(this.tablero[j][j]) === 0){
                        numIgualesDS++;
                    }
                }
                if(i === numTiles-1 && temp && j >0){
                    if(temp.localeCompare(this.tablero[i-j][j]) === 0){
                        numIgualesDI++;
                    }
                }
                if(j>0 && temp2 &&temp2.localeCompare(this.tablero[j][i]) === 0){
                    numIgualesC++;
                }
            }
            if(numIgualesF === numTiles -1 || numIgualesDS === numTiles -1 || numIgualesDI === numTiles -1){
                this.ganador = "Gana " + temp;
                this.turno = -1;
                this.fin = true;
                return;
            }else if(numIgualesC === numTiles -1){
                this.ganador = "Gana " + temp2;
                this.turno = -1;
                this.fin = true;
                return;
            }
        }
        for(var i = 0; i<numTiles;i++){
            for(var j = 0; j<numTiles; j++){
                if(!this.tablero[i][j]){
                    console.log("se sigue");
                    return;
                }
            }
        }
        this.ganador = "Empate";
        this.fin = true;
        console.log("empate");
        this.turno =-1;
    },
    reiniciar(auto){
      this.isAuto= auto;
      console.log(auto);
      this.turno = 1;
      this.ganador = "";
      this.fin = false;
      this.juega = 1;
      for (let i = 0; i < numTiles ; i++){
        for(let j = 0; j< numTiles;j++){
          this.tablero [i][j] = "";
        }
      }
    }
  },
  computed:{
    casillas(){
      return this.tablero;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
td{
    width: 50px;
    height: 50px;
    text-align: center;
    align-content: center;
    border-top: 1px solid #000 ;
    border-bottom: 1px solid #000;
    border-right: 1px solid #000;
    border-left: 1px solid #000;
}
</style>
