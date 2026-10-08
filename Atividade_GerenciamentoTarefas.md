# Gerenciamento de Tarefas com MongoDB

Sistema simples de gerenciamento de tarefas (To-Do List) utilizando **MongoDB** e **MongoShell**.

## Objetivo

Criar e manipular um banco de dados de tarefas, praticando operações de:

- CREATE — Inserção de documentos
- READ — Consulta de documentos
- UPDATE — Atualização de documentos
- DELETE — Exclusão de documentos
- Consultas com filtros e projeções
- Índices
- Agregações

## Banco de Dados

Banco utilizado:

```javascript
use gerenciamentoTarefas
```

Coleção utilizada:

```text
tarefas
```

---

## Exercício 1 - Criação do Banco e Coleção

```javascript
use gerenciamentoTarefas
```

A coleção `tarefas` será criada automaticamente na primeira inserção.

---

## Exercício 2 - Inserção de Tarefas

```javascript
db.tarefas.insertMany([
  {
    titulo: "Comprar Leite",
    descricao: "Leite integral no supermercado",
    status: "pendente",
    prioridade: "alta",
    dataLimite: "2023-10-26"
  },
  {
    titulo: "Preparar Apresentação",
    descricao: "Slides para a reunião de segunda",
    status: "em andamento",
    prioridade: "alta",
    dataLimite: "2023-10-30"
  },
  {
    titulo: "Responder E-mails",
    descricao: "Limpar a caixa de entrada",
    status: "pendente",
    prioridade: "média",
    dataLimite: "2023-10-27"
  }
])
```

---

## Exercício 3 - Leitura de Tarefas

### Listar todas as tarefas

```javascript
db.tarefas.find()
```

### Listar apenas tarefas pendentes

```javascript
db.tarefas.find({
  status: "pendente"
})
```

### Listar tarefas com prioridade alta

Exibir somente o título e a data limite:

```javascript
db.tarefas.find(
  { prioridade: "alta" },
  { _id: 0, titulo: 1, dataLimite: 1 }
)
```

---

## Exercício 4 - Atualização de Tarefas

### Atualizar "Comprar Leite"

Alterar o status para `concluída`:

```javascript
db.tarefas.updateOne(
  { titulo: "Comprar Leite" },
  {
    $set: {
      status: "concluída"
    }
  }
)
```

### Atualizar "Responder E-mails"

Alterar a prioridade para `baixa` e adicionar o responsável:

```javascript
db.tarefas.updateOne(
  { titulo: "Responder E-mails" },
  {
    $set: {
      prioridade: "baixa",
      responsavel: "Seu Nome"
    }
  }
)
```

### Adicionar "Estudar MongoDB"

```javascript
db.tarefas.insertOne({
  titulo: "Estudar MongoDB",
  descricao: "Revisar comandos CRUD e agregação",
  status: "pendente",
  prioridade: "alta",
  dataLimite: "2023-11-05"
})
```

### Adicionar a tag "urgente"

Adicionar a tag a todas as tarefas com status `pendente`:

```javascript
db.tarefas.updateMany(
  { status: "pendente" },
  {
    $set: {
      tag: "urgente"
    }
  }
)
```

---

## Exercício 5 - Deletar Tarefas

### Deletar "Comprar Leite"

```javascript
db.tarefas.deleteOne({
  titulo: "Comprar Leite"
})
```

### Deletar tarefas com prioridade baixa

```javascript
db.tarefas.deleteMany({
  prioridade: "baixa"
})
```

---

## Exercício 6 - Consultas Avançadas e Agregação

### Inserir novas tarefas

```javascript
db.tarefas.insertMany([
  {
    titulo: "Planejar Viagem",
    descricao: "Pesquisar destinos",
    status: "pendente",
    prioridade: "média",
    dataLimite: "2024-01-15"
  },
  {
    titulo: "Pagar Contas",
    descricao: "Contas de água e luz",
    status: "em andamento",
    prioridade: "alta",
    dataLimite: "2023-10-28"
  },
  {
    titulo: "Fazer Exercício",
    descricao: "Academia ou caminhada",
    status: "pendente",
    prioridade: "baixa",
    dataLimite: "2023-10-26"
  }
])
```

### Consultar tarefas pendentes OU com prioridade alta

```javascript
db.tarefas.find({
  $or: [
    { status: "pendente" },
    { prioridade: "alta" }
  ]
})
```

### Criar índice

Criar um índice no campo `status`:

```javascript
db.tarefas.createIndex({
  status: 1
})
```

### Verificar os índices

```javascript
db.tarefas.getIndexes()
```

### Contar tarefas por status

```javascript
db.tarefas.aggregate([
  {
    $group: {
      _id: "$status",
      quantidade: {
        $sum: 1
      }
    }
  }
])
```

### Encontrar a tarefa pendente mais urgente

Ordenar pela data limite em ordem crescente e retornar apenas a primeira:

```javascript
db.tarefas.find({
  status: "pendente"
}).sort({
  dataLimite: 1
}).limit(1)
```

---

## Comandos Completos

Abaixo está a sequência completa dos comandos para execução no MongoShell:

```javascript
use gerenciamentoTarefas

db.tarefas.insertMany([
  {
    titulo: "Comprar Leite",
    descricao: "Leite integral no supermercado",
    status: "pendente",
    prioridade: "alta",
    dataLimite: "2023-10-26"
  },
  {
    titulo: "Preparar Apresentação",
    descricao: "Slides para a reunião de segunda",
    status: "em andamento",
    prioridade: "alta",
    dataLimite: "2023-10-30"
  },
  {
    titulo: "Responder E-mails",
    descricao: "Limpar a caixa de entrada",
    status: "pendente",
    prioridade: "média",
    dataLimite: "2023-10-27"
  }
])

db.tarefas.find()

db.tarefas.find({
  status: "pendente"
})

db.tarefas.find(
  { prioridade: "alta" },
  { _id: 0, titulo: 1, dataLimite: 1 }
)

db.tarefas.updateOne(
  { titulo: "Comprar Leite" },
  {
    $set: {
      status: "concluída"
    }
  }
)

db.tarefas.updateOne(
  { titulo: "Responder E-mails" },
  {
    $set: {
      prioridade: "baixa",
      responsavel: "Seu Nome"
    }
  }
)

db.tarefas.insertOne({
  titulo: "Estudar MongoDB",
  descricao: "Revisar comandos CRUD e agregação",
  status: "pendente",
  prioridade: "alta",
  dataLimite: "2023-11-05"
})

db.tarefas.updateMany(
  { status: "pendente" },
  {
    $set: {
      tag: "urgente"
    }
  }
)

db.tarefas.deleteOne({
  titulo: "Comprar Leite"
})

db.tarefas.deleteMany({
  prioridade: "baixa"
})

db.tarefas.insertMany([
  {
    titulo: "Planejar Viagem",
    descricao: "Pesquisar destinos",
    status: "pendente",
    prioridade: "média",
    dataLimite: "2024-01-15"
  },
  {
    titulo: "Pagar Contas",
    descricao: "Contas de água e luz",
    status: "em andamento",
    prioridade: "alta",
    dataLimite: "2023-10-28"
  },
  {
    titulo: "Fazer Exercício",
    descricao: "Academia ou caminhada",
    status: "pendente",
    prioridade: "baixa",
    dataLimite: "2023-10-26"
  }
])

db.tarefas.find({
  $or: [
    { status: "pendente" },
    { prioridade: "alta" }
  ]
})

db.tarefas.createIndex({
  status: 1
})

db.tarefas.getIndexes()

db.tarefas.aggregate([
  {
    $group: {
      _id: "$status",
      quantidade: {
        $sum: 1
      }
    }
  }
])

db.tarefas.find({
  status: "pendente"
}).sort({
  dataLimite: 1
}).limit(1)
```
