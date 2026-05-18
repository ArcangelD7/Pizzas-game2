
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Juego Memorizar Pizzas</title>

<style>

body{
    margin:0;
    background:#1f1f1f;
    color:white;
    font-family:Arial, sans-serif;
    display:flex;
    justify-content:center;
    align-items:center;
    min-height:100vh;
    padding:20px;
}

.game{
    width:100%;
    max-width:950px;
    background:#2b2b2b;
    padding:35px;
    border-radius:24px;
    box-shadow:0 0 30px rgba(0,0,0,.5);
}

h1{
    text-align:center;
    margin-top:0;
    margin-bottom:30px;
    font-size:2.2rem;
}

.mode-buttons{
    display:flex;
    gap:12px;
    margin-bottom:30px;
}

.mode-buttons button{
    flex:1;
}

.question{
    font-size:1.35rem;
    text-align:center;
    margin-bottom:30px;
    line-height:1.8;
}

.question strong{
    display:block;
    margin-top:15px;
    font-size:1.5rem;
    color:#ffcc70;
}

.answers{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:18px;
}

button{
    border:none;
    padding:18px;
    border-radius:14px;
    background:#444;
    color:white;
    cursor:pointer;
    font-size:1rem;
    transition:.2s;
    text-align:left;
    line-height:1.5;
    font-weight:600;
    letter-spacing:0.3px;
}

button:hover{
    background:#666;
    transform:scale(1.02);
}

.correct{
    background:#1f8b4c !important;
}

.wrong{
    background:#b22222 !important;
}

.next{
    width:100%;
    margin-top:25px;
    background:#d97706;
    text-align:center;
    font-size:1.1rem;
    font-weight:bold;
}

.score{
    text-align:center;
    margin-top:25px;
    font-size:1.3rem;
    font-weight:bold;
}

.pizza-title{
    font-size:1.25rem;
    font-weight:bold;
    margin-bottom:8px;
}

.ingredients-text{
    font-size:0.96rem;
    line-height:1.7;
    color:#f2f2f2;
}

@media(max-width:700px){

    .answers{
        grid-template-columns:1fr;
    }

    h1{
        font-size:1.7rem;
    }

}

</style>
</head>
<body>

<div class="game">

<h1>🍕 Memoriza el Menú Pedro Pepperoni</h1>

<div class="mode-buttons">
    <button onclick="setMode('ingredientes')">
        Ingredientes → Pizza
    </button>

    <button onclick="setMode('nombre')">
        Pizza → Ingredientes
    </button>
</div>

<div class="question" id="question"></div>

<div class="answers" id="answers"></div>

<button class="next" onclick="nextQuestion()">
    Siguiente
</button>

<div class="score" id="score">
    Puntos: 0
</div>

</div>

<script>

const pizzas = [

{
name:"Marinara",
ingredients:"Tomate, orégano, ajo y AOVE"
},

{
name:"Margarita",
ingredients:"Tomate, orégano y mozzarella"
},

{
name:"Toscana",
ingredients:"Tomate, orégano, jamón serrano, cebolla, champiñones y AOVE"
},

{
name:"Prosciutto",
ingredients:"Tomate, orégano, mozzarella y jamón york"
},

{
name:"Funghi",
ingredients:"Tomate, orégano, mozzarella y champiñones"
},

{
name:"Vegana",
ingredients:"Tomate, pimiento rojo, pimiento verde, cebolla, champiñones y berenjena"
},

{
name:"Pepperoni",
ingredients:"Tomate, orégano, mozzarella y pepperoni"
},

{
name:"Cuatro Quesos",
ingredients:"Tomate, orégano, mozzarella, emmental, cheddar y queso azul"
},

{
name:"Hawaiana",
ingredients:"Tomate, orégano, mozzarella, jamón york, piña y champiñones"
},

{
name:"Sorrentina",
ingredients:"Tomate, orégano, mozzarella, atún y cebolla"
},

{
name:"Bianca",
ingredients:"Salsa carbonara, mozzarella, jamón york y champiñones"
},

{
name:"Diavola",
ingredients:"Tomate, orégano, mozzarella, chorizo picante, champiñones y cebolla"
},

{
name:"Carbonara",
ingredients:"Salsa carbonara, orégano, mozzarella, cebolla y bacon"
},

{
name:"Cremosa",
ingredients:"Salsa barbacoa y carbonara, mozzarella, jamón york y bacon"
},

{
name:"Barbacoa",
ingredients:"Salsa barbacoa, mozzarella, jamón york, bacon, serrano y pollo"
},

{
name:"Mexicana",
ingredients:"Tomate, orégano, mozzarella, tabasco, carne picada, aceitunas negras, cebolla y jalapeño"
},

{
name:"Trastévere",
ingredients:"Salsa carbonara, cuatro quesos, pollo y pepperoni"
},

{
name:"Puttanesca",
ingredients:"Tomate, orégano, mozzarella, anchoas, tomate cherry, atún, aceitunas negras y alcaparras"
},

{
name:"Rústica",
ingredients:"Tomate, orégano, mozzarella, york, bacon, cebolla, aceitunas negras, pimiento verde y rojo"
},

{
name:"Cuatro Estaciones",
ingredients:"Tomate, orégano, mozzarella, champiñones, alcachofas, york y aceitunas negras"
},

{
name:"De la Huerta",
ingredients:"Tomate, mozzarella, berenjenas, pimiento rojo y verde, verduras de temporada, tomate cherry confitado y AOVE"
},

{
name:"Piero",
ingredients:"Tomate, orégano, mozzarella, york, atún, champiñones y cebolla"
},

{
name:"Ibérica",
ingredients:"Tomate, mozzarella, jamón serrano, rúcula, tomate cherry y parmesano en lascas"
},

{
name:"Speck",
ingredients:"Tomate, mozzarella, speck, tomate cherry, rúcula y AOVE"
},

{
name:"Capricciosa",
ingredients:"Tomate, orégano, mozzarella, york, champiñones, aceitunas negras, alcachofas y anchoas"
},

{
name:"Porchetta",
ingredients:"Tomate, mozzarella, porchetta y pepperoni"
},

{
name:"Foie",
ingredients:"Tomate, orégano, mozzarella, foie, queso de cabra y cebolla caramelizada"
},

{
name:"Bufalina",
ingredients:"Tomate, orégano, mozzarella di bufala campana, aceitunas negras y albahaca"
},

{
name:"Burrata",
ingredients:"Tomate, burrata, tomate seco, miel de caña, albahaca y AOVE"
},

{
name:"Parmigiana",
ingredients:"Base tomate San Marzano D.O.P., mozzarella, berenjena frita, boloñesa, parmesano y albahaca"
},

{
name:"De la Casa",
ingredients:"Salsa casera Pedro Pepperoni, mozzarella, bacon y queso de cabra"
},

{
name:"Calabresa",
ingredients:"Base tomate San Marzano D.O.P., mozzarella, spianatta calabresa, tomate cherry confitado y aceitunas negras"
},

{
name:"Guanciale",
ingredients:"Mozzarella, guanciale, parmesano, yema de huevo, pimienta negra y albahaca"
},

{
name:"Nduja",
ingredients:"Mozzarella, scamorza affumicata, nduja, nuez picada, miel, burrata y albahaca"
},

{
name:"Gorgonzola",
ingredients:"Nata, mozzarella, gorgonzola, pera confitada, nueces, miel y albahaca"
},

{
name:"Fuoco",
ingredients:"Crema de calabaza, spianatta calabresa, huevo y albahaca"
},

{
name:"Zucca",
ingredients:"Crema de calabaza, mozzarella, guanciale y albahaca"
},

{
name:"Arrabbiata",
ingredients:"Tomate San Marzano D.O.P., spianatta calabresa, nduja, burrata y albahaca"
},

{
name:"Pesto",
ingredients:"Mozzarella, mortadela de pistacho al horno, perlas de mozzarella, tomate cherry, pesto y albahaca"
},

{
name:"Pistaccio",
ingredients:"Mozzarella, mortadela de pistacho, burrata, pistacho troceado, tomate cherry, AOVE y albahaca"
},

{
name:"Tartufata",
ingredients:"Base de tartufata, mozzarella, champiñones, burrata y albahaca"
},

{
name:"Cacio e Pepe",
ingredients:"Salsa cacio e pepe, mozzarella, guanciale, carne picada y albahaca"
},

{
name:"Porcini",
ingredients:"Crema de boletus, scamorza ahumada, porchetta, burrata al tartufo y albahaca"
}

];

let score = 0;
let currentPizza = null;
let mode = "ingredientes";

function setMode(newMode){
    mode = newMode;
    nextQuestion();
}

function shuffle(array){
    return array.sort(() => Math.random() - 0.5);
}

function nextQuestion(){

    const answersDiv = document.getElementById("answers");
    answersDiv.innerHTML = "";

    currentPizza = pizzas[
        Math.floor(Math.random() * pizzas.length)
    ];

    const wrongAnswers = shuffle(
        pizzas.filter(p => p.name !== currentPizza.name)
    ).slice(0,3);

    const options = shuffle([
        currentPizza,
        ...wrongAnswers
    ]);

    const question = document.getElementById("question");

    if(mode === "ingredientes"){

        question.innerHTML = `
        ¿Qué pizza tiene estos ingredientes?
        <strong>${currentPizza.ingredients}</strong>
        `;

    }else{

        question.innerHTML = `
        ¿Qué ingredientes lleva esta pizza?
        <strong>🍕 ${currentPizza.name}</strong>
        `;
    }

    options.forEach(option => {

        const btn = document.createElement("button");

        if(mode === "ingredientes"){

            btn.innerHTML = `
                <div class="pizza-title">
                    🍕 ${option.name}
                </div>
            `;

        }else{

            btn.innerHTML = `
                <div class="ingredients-text">
                    ${option.ingredients}
                </div>
            `;
        }

        btn.onclick = () => checkAnswer(btn, option);

        answersDiv.appendChild(btn);

    });

}

function checkAnswer(button, option){

    const buttons =
        document.querySelectorAll(".answers button");

    buttons.forEach(b => b.disabled = true);

    if(option.name === currentPizza.name){

        button.classList.add("correct");
        score++;

    }else{

        button.classList.add("wrong");

        buttons.forEach(b => {

            if(
                (mode === "ingredientes" &&
                b.innerText.includes(currentPizza.name))

                ||

                (mode === "nombre" &&
                b.innerText.includes(currentPizza.ingredients))
            ){

                b.classList.add("correct");

            }

        });

    }

    document.getElementById("score")
    .textContent = "Puntos: " + score;

}

nextQuestion();

</script>

</body>
</html>
