<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Contador de Passos</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Contador de Passos</h1>
    <div id="contador">0</div>
    <button id="incrementar">Adicionar Passo</button>
    <button id="resetar">Resetar Contador</button>
    <div id="historico"></div>
    <div id="meta">
        <label for="metaDiaria">Defina sua meta diária de passos:</label>
        <input type="number" id="metaDiaria" value="1000">
    </div>
    <button id="gerarRelatorio" class="gerar-relatorio">Gerar Relatório</button>
    <div id="relatorio"></div>

    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background-color: #f0f0f0;
}

#contador {
    font-size: 48px;
    margin: 20px;
    padding: 20px;
    border-radius: 10px;
    background-color: #fff;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

button {
    padding: 10px 20px;
    font-size: 20px;
    cursor: pointer;
    margin: 5px;
    color: white;
    border: none;
    border-radius: 5px;
}

#incrementar {
    background-color: #4CAF50;
}

#resetar {
    background-color: #f44336;
}

#historico {
    margin-top: 20px;
    max-width: 300px;
    text-align: left;
}

#meta {
    margin-top: 20px;
}

.gerar-relatorio {
    color: #1a0dab; /* Cor azul semelhante a um link do Google */
    background-color: transparent; /* Fundo transparente */
    border: none; /* Remove a borda */
    text-decoration: underline; /* Adiciona sublinhado */
}
let passos = 0;
let historico = [];
let metaDiaria = 1000; // Meta padrão
let atividades = [];

// Função para atualizar o contador na tela
function atualizarContador() {
    document.getElementById('contador').innerText = passos;
    document.getElementById('historico').innerText = "Histórico: " + historico.join(", ");
}

// Evento de clique no botão para simular passos
document.getElementById('incrementar').addEventListener('click', function() {
    passos++;
    historico.push(passos);
    atividades.push({ data: new Date(), tipo: 'Passo' });
    atualizarContador();
    verificarMeta();
});

// Evento de clique no botão para resetar o contador
document.getElementById('resetar').addEventListener('click', function() {
    passos = 0;
    historico = [];
    atividades = [];
    atualizarContador();
});

// Função para verificar se a meta foi atingida
function verificarMeta() {
    if (passos >= metaDiaria) {
        alert("Parabéns! Você atingiu sua meta de " + metaDiaria + " passos!");
    }
}

// Evento para definir a meta diária
document.getElementById('metaDiaria').addEventListener('change', function() {
    metaDiaria = parseInt(this.value);
});

// Função para gerar relatórios
document.getElementById('gerarRelatorio').addEventListener('click', function() {
    const relatorio = atividades.map(atividade => {
        return `Data: ${atividade.data.toLocaleString()}, Tipo: ${atividade.tipo}`;
    }).join("<br>");
    document.getElementById('relatorio').innerHTML = relatorio || "Nenhuma atividade registrada.";
});

// Função para detectar passos em dispositivos móveis
if (window.DeviceMotionEvent) {
    window.addEventListener('devicemotion', function(event) {
        const acceleration = event.acceleration;
        if (acceleration.x > 1 || acceleration.y > 1 || acceleration.z > 1) {
            passos++;
            historico.push(passos);
            atividades.push({ data: new Date(), tipo: 'Passo' });
            atualizarContador();
            verificarMeta();
        }
    });
} else {
    alert("Este dispositivo não suporta a API de movimento.");
}
