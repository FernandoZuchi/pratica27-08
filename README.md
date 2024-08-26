# Prática da semana - Calculadora de Bitcoin

Bem-vindo a este projeto divertido e prático onde construiremos uma calculadora que converte Bitcoin em USD. Você aprenderá sobre como buscar dados de uma API, manipulação do DOM, e uso de localStorage, além de utilizar um pouco de matemática básica ao longo do caminho.

Ao final deste tutorial, você terá uma calculadora de preços de Bitcoin funcional que busca o preço atual do Bitcoin, permite calcular o valor de suas posses em Bitcoin, e salva seus dados localmente.

Este é um projeto amigável para iniciantes, adequado para aqueles que entendem o básico de HTML, CSS e JavaScript puro.

## Configuração do Projeto

Crie uma nova pasta e dê um nome relevante ao projeto, como btc-usd-calc. Crie um arquivo chamado index.html na pasta do seu projeto.

Comece escrevendo o HTML. A página deve conter essencialmente:

- Um título
- O preço atual do Bitcoin
- Um campo de texto para o usuário inserir suas posses em Bitcoin 
- Um botão para calcular o valor de suas posses em Bitcoin
- O valor calculado

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Bitcoin</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Calculadora de Bitcoin</h1>
        <main id="main">
            <p>
                <span id="precoAtual">Preço mais recente: </span>
                <b>$<span id="precoBitcoin"></span> USD (Dólar)</b>
            </p>
            <label for="qtadeBitcoin">Quantos Bitcoins você possui?</label>
            <input type="number" id="qtadeBitcoin" step="any" placeholder="0.05">
            <button id="calcularBitcoin">Calcular</button>
            <br/>
            <br/>
            <h3 id="qtadeUSD"></h3>
        </main>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

## Buscando dados da API

Agora, no mesmo diretório do index.html, crie um novo arquivo chamado script.js.

No script.js, defina uma constante para a URL da API:
```javascript
  const API_URL = "https://api.coindesk.com/v1/bpi/currentprice.json";
```
A próxima coisa a fazer é adicionar um event listener para chamar a API assim que o conteúdo da página for carregado. Você pode fazer isso adicionando o código abaixo diretamente abaixo da constante API_URL:

```javascript
  document.addEventListener("DOMContentLoaded", async () => {
  // Executa uma função assíncrona assim que o conteúdo do DOM for carregado
  let precoBitcoin; // Inicializa a variável

  try {
    const response = await fetch(API_URL);
    // Aguarda uma resposta da solicitação HTTP enviada à URL da API

    const data = await response.json();
    // Aguarda o conteúdo JSON da resposta

    precoBitcoin = data.bpi.USD.rate_float.toFixed(2);
    // Atribui à variável bitcoinPrice o valor em USD do Bitcoin, arredondado para 2 casas decimais.
  } catch {
    console.log("erro!"); // Em caso de erro
  }

  console.log(precoBitcoin); // Exibe o preço no console
});
```

Em caso de algum erro, o programa nos avisará no console. Caso contrário, o preço do Bitcoin será exibido no console. Você pode testar agora e ver se consegue obter o preço atual do Bitcoin!

## Como Implementar o localStorage
Fundamentalmente, o localStorage é um recurso presente na maioria dos navegadores que salvam informações para que sejam mantidas na memória do navegador mesmo após a página ou o navegador serem fechados.

Vamos começar editando algumas linhas:
```javascript
const API_URL = "https://api.coindesk.com/v1/bpi/currentprice.json";

document.addEventListener("DOMContentLoaded", async () => {
    // Executa uma função assíncrona assim que o conteúdo do DOM for carregado
    let precoBitcoin = localStorage.getItem("precoBitcoin"); // Inicializa a variável
  
    try {
      const response = await fetch(API_URL);
      // Aguarda uma resposta da solicitação HTTP enviada à URL da API
  
      const data = await response.json();
      // Aguarda o conteúdo JSON da resposta
  
      precoBitcoin = data.bpi.USD.rate_float.toFixed(2);
      localStorage.setItem("precoBitcoin", precoBitcoin);
      // Atribui à variável precoBitcoin o valor em USD do Bitcoin, arredondado para 2 casas decimais.
    } catch {
      console.log("erro!"); // Em caso de erro
    }
  
    console.log(precoBitcoin); // Exibe o preço no console
  });
```

## Como Implementar Manipulação do DOM
O Document Object Model (DOM) é uma interface que permite interações de programação com documentos web. Essencialmente, a manipulação do DOM com JavaScript nos permite atualizar certas partes ou todo um documento.

Neste projeto, usaremos o DOM para mostrar o preço atual do Bitcoin e o valor calculado. Também usaremos para recuperar o valor inserido pelo usuário no campo de texto #bitcoinAmount quando o botão de cálculo for clicado.

Vamos implementar a manipulação do DOM:
```javascript
const API_URL = "https://api.coindesk.com/v1/bpi/currentprice.json";

document.addEventListener("DOMContentLoaded", async () => {
    const main = document.getElementById("main");
    const elementoPreco = document.getElementById("precoBitcoin");
    const qtadeBitcoinInput = document.getElementById("qtadeBitcoin");
    const btnCalcular = document.getElementById("calcularBitcoin");
    const qtadeDolar = document.getElementById("qtadeUSD");
    // Executa uma função assíncrona assim que o conteúdo do DOM for carregado
    let precoBitcoin = localStorage.getItem("precoBitcoin"); // Inicializa a variável
  
    try {
      const response = await fetch(API_URL);
      // Aguarda uma resposta da solicitação HTTP enviada à URL da API
  
      const data = await response.json();
      // Aguarda o conteúdo JSON da resposta
  
      precoBitcoin = data.bpi.USD.rate_float.toFixed(2);
      localStorage.setItem("precoBitcoin", precoBitcoin);
      // Atribui à variável precoBitcoin o valor em USD do Bitcoin, arredondado para 2 casas decimais.
    } catch {
      if(precoBitcoin) {
        // Se houver um preço de bitcoin existente no localStorage
        elementoPreco.textContent = precoBitcoin; // Exibe o que está salvo lá
      }
      else {
        main.innerHTML = "<p>Não foi possível buscar o preço do bitcoin</p>"
      }
    }
  
    console.log(precoBitcoin); // Exibe o preço no console
  });
```

## Como Calcular o Valor Atual da Carteira
Agora, o objetivo de uma calculadora de Bitcoin é calcular quanto vale a carteira de Bitcoin de alguém, não necessariamente qual é o preço atual.

Por exemplo, o preço atual do Bitcoin pode ser $60.000 USD. Se você possui 2 Bitcoins, sua carteira vale $120.000 USD. Se você possui metade (0,5) de um Bitcoin, sua carteira vale $30.000 USD.

Vamos obter a quantidade de Bitcoin que o usuário possui (bitcoinAmount) do localStorage.
```javascript
// continue após console.log(precoBitcoin);

let qtadeBitcoin = localStorage.getItem("qtadeBitcoin");
```
Calcule o valor em USD com esta função:

```javascript
const calcularMontanteUSD = () => {
  qtadeBitcoin = qtadeBitcoinInput.value || 0;
  // qtadeBitcoin será reatribuído para o que estiver no input no front-end, caso contrário, seu valor padrão será zero

  const totalUSD = qtadeBitcoin * precoBitcoin;
  // Suponha que você tenha 2 Bitcoins e o preço seja 60000.
  // 2 * 60000 = 120000

  qtadeDolar.innerHTML = `<b>$${totalUSD.toFixed(
    2
  )} USD</b> em Bitcoin.`;
  // Arredonde para as duas casas decimais mais próximas e exiba
};
```

Vamos verificar a existência de um valor inserido pelo usuário, e calcular em cima disso

```javascript
if (qtadeBitcoinInput.value) {
        calcularMontanteUSD()
    }
```

Para que o usuário atualize seu bitcoinAmount, vamos adicionar um event listener quando o botão calculateBtn for clicado:

```javascript
btnCalcular.addEventListener("click", () => {
        localStorage.setItem("qtadeBitcoin", qtadeBitcoinInput.value);
        calcularMontanteUSD();
    });
```

Todo o JavaScript agora deve estar completo.

## Código Completo em JavaScript
Seu código completo deve ser semelhante a este (além dos comentários e formatação):
```javascript
const API_URL = "https://api.coindesk.com/v1/bpi/currentprice.json";

document.addEventListener("DOMContentLoaded", async () => {
    let main = document.getElementById("main");
    let elementoPreco = document.getElementById("precoBitcoin");
    let qtadeBitcoinInput = document.getElementById("qtadeBitcoin");
    let btnCalcular = document.getElementById("calcularBitcoin");
    let qtadeDolar = document.getElementById("qtadeUSD");
    // Executa uma função assíncrona assim que o conteúdo do DOM for carregado
    let precoBitcoin = localStorage.getItem("precoBitcoin"); // Inicializa a variável

    try {
        const response = await fetch(API_URL);
        // Aguarda uma resposta da solicitação HTTP enviada à URL da API

        const data = await response.json();
        // Aguarda o conteúdo JSON da resposta

        precoBitcoin = data.bpi.USD.rate_float.toFixed(2);
        localStorage.setItem("precoBitcoin", precoBitcoin);
        // Atribui à variável precoBitcoin o valor em USD do Bitcoin, arredondado para 2 casas decimais.
    } catch {
        if (precoBitcoin) {
            // Se houver um preço de bitcoin existente no localStorage
            elementoPreco.textContent = precoBitcoin; // Exibe o que está salvo lá
        }
        else {
            main.innerHTML = "<p>Não foi possível buscar o preço do bitcoin</p>"
        }
    }

    console.log(precoBitcoin); // Exibe o preço no console

    qtadeBitcoin = localStorage.getItem("qtadeBitcoin");

    const calcularMontanteUSD = () => {
        qtadeBitcoin = parseFloat(qtadeBitcoinInput.value) || 0;
        // qtadeBitcoin será reatribuído para o que estiver no input no front-end, caso contrário, seu valor padrão será zero

        const totalUSD = qtadeBitcoin * precoBitcoin;
        // Suponha que você tenha 2 Bitcoins e o preço seja 60000.
        // 2 * 60000 = 120000

        qtadeDolar.innerHTML = `<b>$${totalUSD.toFixed(
            2
        )} USD</b> em Bitcoin.`;
        // Arredonde para as duas casas decimais mais próximas e exiba
    };

    if (qtadeBitcoinInput.value) {
        calcularMontanteUSD()
    }

    btnCalcular.addEventListener("click", () => {
        localStorage.setItem("qtadeBitcoin", qtadeBitcoinInput.value);
        calcularMontanteUSD();
    });
});


```

Com isso, sua calculadora estará pronta! Agora, teste a página em seu navegador e verifique se tudo está funcionando como esperado.

## Estilização do Projeto (CSS)
Vamos aplicar um estilo básico para melhorar a aparência do projeto. Adicione o código CSS a seguir:
```css
body {
  font-family: Arial, sans-serif;
  background: #f0f0f0;
  color: #333;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

.container {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
  width: 300px;
  text-align: center;
}

input {
  margin-top: 0.5rem;
  padding: 0.5rem;
  font-size: 1rem;
  width: 100%;
  box-sizing: border-box;
}

button {
  margin-top: 1rem;
  padding: 0.5rem;
  font-size: 1rem;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
```
## Conclusão
Parabéns! Você agora tem uma calculadora de Bitcoin que não só busca o preço atual de Bitcoin, mas também calcula e exibe o valor em USD das posses de Bitcoin que o usuário insere. Além disso, você aprendeu a usar o localStorage para armazenar dados localmente e a manipular o DOM com JavaScript puro.

Agora é hora de adicionar funcionalidades ou personalizações ao projeto! Por exemplo, você pode adicionar um seletor de moeda ou exibir gráficos de preços históricos. As possibilidades são infinitas!
