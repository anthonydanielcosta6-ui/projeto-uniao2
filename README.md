<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<title>Sistema Escolar</title>

<style>

body{
font-family:Arial;
background:#f0f2f5;
padding:20px;
}

.card{
background:white;
padding:20px;
border-radius:10px;
box-shadow:0 2px 10px rgba(0,0,0,.1);
max-width:800px;
margin:auto;
}

input{
width:100%;
padding:10px;
margin:5px 0;
}

button{
padding:10px 20px;
cursor:pointer;
}

table{
width:100%;
margin-top:20px;
border-collapse:collapse;
}

th,td{
border:1px solid #ddd;
padding:8px;
}

tr:nth-child(even){
background:#f5f5f5;
}

</style>
</head>

<body>

<div class="card">

<h2>Sistema Escolar</h2>

<input id="nome" placeholder="Nome do aluno">

<input id="turma" placeholder="Turma">

<button onclick="cadastrar()">
Cadastrar
</button>

<table>

<thead>
<tr>
<th>Nome</th>
<th>Turma</th>
<th>Ação</th>
</tr>
</thead>

<tbody id="lista"></tbody>

</table>

</div>

<script>

const API =
"https://script.google.com/macros/s/AKfycbwH_DGTAm6Cbc7jtYZvudizi6SDu1SZ_m0wAFvZsTL2ZABCI0HAKYSYRWoYIlCKWD0D/exec";

async function cadastrar(){

const nome =
document.getElementById("nome").value;

const turma =
document.getElementById("turma").value;

await fetch(API,{
method:"POST",
body:JSON.stringify({
acao:"cadastrar",
nome,
turma
})
});

listar();
}

async function listar(){

const resposta =
await fetch(API + "?acao=listar");

const alunos =
await resposta.json();

let html = "";

alunos.forEach(aluno=>{

html += `
<tr>
<td>${aluno.nome}</td>
<td>${aluno.turma}</td>
<td>
<button onclick="excluir('${aluno.id}')">
Excluir
</button>
</td>
</tr>
`;

});

document.getElementById("lista").innerHTML = html;
}

async function excluir(id){

await fetch(API,{
method:"POST",
body:JSON.stringify({
acao:"excluir",
id
})
});

listar();
}

listar();

</script>

</body>
</html>
