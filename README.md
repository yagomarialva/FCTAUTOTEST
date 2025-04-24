# Sistema de Gestão de Monitores ESD

Este projeto é um sistema para testes ESD, com gerenciamento de **linhas**, **estações** e **monitores**, integrando também componentes físicos como **pulseiras** e **jigs** com microcontroladores.

---

## Estrutura do Projeto

O projeto está organizado nas seguintes pastas principais:

### Backend (`./backend`)
Contém a API desenvolvida em C# para gerenciar os dados e a lógica do sistema.

- **Controllers**: Controladores responsáveis por expor os endpoints da API.
- **Models**: Modelos de dados que representam as tabelas do banco de dados.
- **Repositories**: Implementações para acesso ao banco de dados.
- **Services**: Camada de lógica de negócios.
- **Hubs**: Configuração do SignalR para comunicação em tempo real.
- **Middleware**: Configurações de middlewares personalizados.
- **SwaggerSettings**: Configuração do Swagger para documentação da API.

### Frontend (`./frontend`)
Contém a interface do usuário desenvolvida em React.

- **src/components**: Componentes reutilizáveis, como tabelas, formulários e modais.
  - **ESD**: Componentes específicos para o gerenciamento de monitores, jigs, estações e operadores.
- **src/pages**: Páginas principais do sistema.
  - **DashboardPage**: Página inicial do sistema.
  - **ESD**: Páginas relacionadas ao gerenciamento de monitores e estações.
  - **LoginPage**: Página de login.
  - **SignUpPage**: Página de cadastro de usuários.
- **src/api**: Configuração das chamadas à API.
- **src/context**: Gerenciamento de estado global com Context API.

### Embarcados (`./embarcados`)
Contém o código para os dispositivos físicos (pulseiras e jigs) desenvolvidos em C++ para Arduino.

- **1ESD_Monitor**: Código para o monitor ESD.
  - **main**: Código principal do monitor.
- **2ESD_BaseJig**: Código para o jig ESD.
  - **main**: Código principal do jig.

### Outros Diretórios

- **./init-scripts**: Scripts de inicialização e configuração.
- **./.vscode**: Configurações específicas para o Visual Studio Code.
- **./.git**: Arquivos de controle de versão do Git.

---

## Endpoints da API

#### 1. AuthenticationController
- **Rota:** `POST /api/Authentication`
- **Descrição:** Realiza login e retorna um token JWT.
- 
#### 2. StationController
### **1. Listar todos os Jigs**
- **Rota:** `GET /api/Jig/todosJigs`
- **Descrição:** Retorna todos os Jigs cadastrados.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/Jig/todosJigs \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
[
  {
    "id": 1,
    "name": "Jig 1",
    "serialNumber": "SN12345",
    "created": "2025-04-16T10:00:00",
    "lastUpdated": "2025-04-16T12:00:00"
  },
  {
    "id": 2,
    "name": "Jig 2",
    "serialNumber": "SN67890",
    "created": "2025-04-16T11:00:00",
    "lastUpdated": "2025-04-16T13:00:00"
  }
]
```

---

### **2. Buscar Jig por ID**
- **Rota:** `GET /api/Jig/buscarJig/{id}`
- **Descrição:** Retorna um Jig específico pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/Jig/buscarJig/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "name": "Jig 1",
  "serialNumber": "SN12345",
  "created": "2025-04-16T10:00:00",
  "lastUpdated": "2025-04-16T12:00:00"
}
```

---

### **3. Buscar Jig por Serial Number**
- **Rota:** `GET /api/Jig/jig-bysn/{sn}`
- **Descrição:** Retorna um Jig específico pelo Serial Number.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/Jig/jig-bysn/SN12345 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "name": "Jig 1",
  "serialNumber": "SN12345",
  "created": "2025-04-16T10:00:00",
  "lastUpdated": "2025-04-16T12:00:00"
}
```

---

### **4. Criar ou atualizar um Jig**
- **Rota:** `POST /api/Jig/gerenciarJigs`
- **Descrição:** Adiciona um novo Jig ou atualiza um Jig existente.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X POST http://localhost:5000/api/Jig/gerenciarJigs \
-H "Content-Type: application/json" \
-H "Authorization: Bearer <TOKEN>" \
-d '{
  "id": 1,
  "name": "Jig Atualizado",
  "serialNumber": "SN12345"
}'
```
- **Exemplo de resposta (atualização):**
```json
{
  "id": 1,
  "name": "Jig Atualizado",
  "serialNumber": "SN12345",
  "created": "2025-04-16T10:00:00",
  "lastUpdated": "2025-04-16T14:00:00"
}
```
- **Exemplo de resposta (adição):**
```json
{
  "id": 3,
  "name": "Novo Jig",
  "serialNumber": "SN54321",
  "created": "2025-04-16T15:00:00",
  "lastUpdated": "2025-04-16T15:00:00"
}
```

---

### **5. Deletar um Jig**
- **Rota:** `DELETE /api/Jig/deleteJigs/{id}`
- **Descrição:** Deleta um Jig específico pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X DELETE http://localhost:5000/api/Jig/deleteJigs/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "name": "Jig 1",
  "serialNumber": "SN12345"
}
```

---

## Observações
- Substitua `<TOKEN>` pelo token JWT obtido no endpoint de autenticação.
- Certifique-se de que o servidor está rodando na porta correta (exemplo: `http://localhost:5000`).
- Para endpoints protegidos, é necessário incluir o cabeçalho `Authorization` com o token JWT.
- O endpoint de adicionar ou atualizar (`POST /api/Jig/gerenciarJigs`) verifica se o ID já existe:
  - Se o ID existir, o Jig será atualizado.
  - Se o ID não existir, um novo Jig será criado.
#### 4. LinkStationAndLine
## Relação entre `Line` e `Station`

- **Line (Linha):** Representa uma linha de produção ou uma unidade lógica no sistema.
- **Station (Estação):** Representa uma estação de trabalho ou um ponto específico dentro de uma linha.
- **LinkStationAndLine:** Representa a relação entre uma linha e uma estação, permitindo associar várias estações a uma linha e vice-versa.

---

## Endpoints Disponíveis

### **1. Listar todos os links**
- **Rota:** `GET /api/LinkStationAndLine/todosLinks`
- **Descrição:** Retorna todos os links cadastrados entre linhas e estações.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/LinkStationAndLine/todosLinks \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
[
  {
    "id": 1,
    "lineID": 1,
    "stationID": 2,
    "line": {
      "id": 1,
      "name": "Linha 1"
    },
    "station": {
      "id": 2,
      "name": "Estação 2"
    }
  }
]
```

---

### **2. Buscar link por ID**
- **Rota:** `GET /api/LinkStationAndLine/{id}`
- **Descrição:** Retorna um link específico pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/LinkStationAndLine/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "lineID": 1,
  "stationID": 2,
  "line": {
    "id": 1,
    "name": "Linha 1"
  },
  "station": {
    "id": 2,
    "name": "Estação 2"
  }
}
```

---

### **3. Buscar links por linha ID**
- **Rota:** `GET /api/LinkStationAndLine/linha-id/{linhaId}`
- **Descrição:** Retorna todos os links associados a uma linha específica.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/LinkStationAndLine/linha-id/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
[
  {
    "id": 1,
    "lineID": 1,
    "stationID": 2,
    "line": {
      "id": 1,
      "name": "Linha 1"
    },
    "station": {
      "id": 2,
      "name": "Estação 2"
    }
  }
]
```

---

### **4. Buscar links por estação ID**
- **Rota:** `GET /api/LinkStationAndLine/estacao-id/{id}`
- **Descrição:** Retorna todos os links associados a uma estação específica.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/LinkStationAndLine/estacao-id/2 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
[
  {
    "id": 1,
    "lineID": 1,
    "stationID": 2,
    "line": {
      "id": 1,
      "name": "Linha 1"
    },
    "station": {
      "id": 2,
      "name": "Estação 2"
    }
  }
]
```

---

### **5. Criar ou atualizar um link**
- **Rota:** `POST /api/LinkStationAndLine/criarLink`
- **Descrição:** Cria ou atualiza um link entre uma linha e uma estação.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X POST http://localhost:5000/api/LinkStationAndLine/criarLink \
-H "Content-Type: application/json" \
-H "Authorization: Bearer <TOKEN>" \
-d '{
  "lineID": 1,
  "stationID": 2
}'
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "lineID": 1,
  "stationID": 2
}
```

---

### **6. Deletar um link**
- **Rota:** `DELETE /api/LinkStationAndLine/{id}`
- **Descrição:** Deleta um link específico pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X DELETE http://localhost:5000/api/LinkStationAndLine/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "lineID": 1,
  "stationID": 2
}
```

---

## Observações
- Substitua `<TOKEN>` pelo token JWT obtido no endpoint de autenticação.
- Certifique-se de que o servidor está rodando na porta correta (exemplo: `http://localhost:5000`).
- Para endpoints protegidos, é necessário incluir o cabeçalho `Authorization` com o token JWT.
- O endpoint de criar ou atualizar (`POST /api/LinkStationAndLine/criarLink`) verifica se a combinação de `lineID` e `stationID` já existe:
  - Se existir, retorna um erro.
  - Caso contrário, cria um novo link.

---

## Relação com `Line` e `Station`
- Cada link conecta uma linha (`Line`) a uma estação (`Station`).
- A API utiliza os repositórios de `Line` e `Station` para validar os IDs fornecidos e garantir que as referências sejam válidas.
- O método `PopulateLinkDetailsAsync` no serviço adiciona os detalhes da linha e da estação ao link, retornando informações completas no JSON de resposta.
### . LogMonitorEsdController
- **Rota:** `GET /api/LogMonitorEsd/ListMonitorEsd`
- **Descrição:** Retorna uma lista de logs de monitores ESD.

#### 5. HubController
- **Rota:** `POST /api/Hub/send-log`
- **Descrição:** Envia um log para o Hub SignalR.

#### MonitorEsdController
### **1. Listar todos os monitores ESD**
- **Rota:** `GET /api/MonitorEsd/todosmonitores`
- **Descrição:** Retorna todos os monitores ESD cadastrados.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/MonitorEsd/todosmonitores \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
[
  {
    "id": 1,
    "serialNumberEsp": "SN12345",
    "description": "Monitor 1",
    "status": 1,
    "created": "2025-04-16T10:00:00",
    "lastUpdated": "2025-04-16T12:00:00"
  },
  {
    "id": 2,
    "serialNumberEsp": "SN67890",
    "description": "Monitor 2",
    "status": 0,
    "created": "2025-04-16T11:00:00",
    "lastUpdated": "2025-04-16T13:00:00"
  }
]
```

---

### **2. Buscar monitor ESD por ID**
- **Rota:** `GET /api/MonitorEsd/{id}`
- **Descrição:** Retorna um monitor ESD específico pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/MonitorEsd/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "serialNumberEsp": "SN12345",
  "description": "Monitor 1",
  "status": 1,
  "created": "2025-04-16T10:00:00",
  "lastUpdated": "2025-04-16T12:00:00"
}
```

---

### **3. Buscar monitor ESD por Serial Number**
- **Rota:** `GET /api/MonitorEsd/Pesquisa/{sn}`
- **Descrição:** Retorna um monitor ESD específico pelo Serial Number.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/MonitorEsd/Pesquisa/SN12345 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "serialNumberEsp": "SN12345",
  "description": "Monitor 1",
  "status": 1,
  "created": "2025-04-16T10:00:00",
  "lastUpdated": "2025-04-16T12:00:00"
}
```

---

### **4. Criar ou atualizar um monitor ESD**
- **Rota:** `POST /api/MonitorEsd/monitores`
- **Descrição:** Adiciona um novo monitor ESD ou atualiza um monitor existente.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X POST http://localhost:5000/api/MonitorEsd/monitores \
-H "Content-Type: application/json" \
-H "Authorization: Bearer <TOKEN>" \
-d '{
  "id": 1,
  "serialNumberEsp": "SN12345",
  "description": "Monitor Atualizado",
  "status": 1
}'
```
- **Exemplo de resposta (atualização):**
```json
{
  "id": 1,
  "serialNumberEsp": "SN12345",
  "description": "Monitor Atualizado",
  "status": 1,
  "created": "2025-04-16T10:00:00",
  "lastUpdated": "2025-04-16T14:00:00"
}
```
- **Exemplo de resposta (adição):**
```json
{
  "id": 3,
  "serialNumberEsp": "SN54321",
  "description": "Novo Monitor",
  "status": 0,
  "created": "2025-04-16T15:00:00",
  "lastUpdated": "2025-04-16T15:00:00"
}
```

---

### **5. Deletar um monitor ESD**
- **Rota:** `DELETE /api/MonitorEsd/{id}`
- **Descrição:** Deleta um monitor ESD específico pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X DELETE http://localhost:5000/api/MonitorEsd/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "serialNumberEsp": "SN12345",
  "description": "Monitor 1"
}
```

---

## Observações
- Substitua `<TOKEN>` pelo token JWT obtido no endpoint de autenticação.
- Certifique-se de que o servidor está rodando na porta correta (exemplo: `http://localhost:5000`).
- Para endpoints protegidos, é necessário incluir o cabeçalho `Authorization` com o token JWT.
- O endpoint de criar ou atualizar (`POST /api/MonitorEsd/monitores`) verifica se o ID já existe:
  - Se o ID existir, o monitor será atualizado.
  - Se o ID não existir, um novo monitor será criado.
- O método de exclusão (`DELETE /api/MonitorEsd/{id}`) também remove associações relacionadas ao monitor na tabela `StationView`.

#### LineController
### **1. Listar todas as linhas**
- **Rota:** `GET /api/Line/TodasLinhas`
- **Descrição:** Retorna todas as linhas cadastradas.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/Line/TodasLinhas \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
[
  {
    "id": 1,
    "name": "Linha 1",
    "created": "2025-04-16T10:00:00",
    "lastUpdated": "2025-04-16T12:00:00"
  },
  {
    "id": 2,
    "name": "Linha 2",
    "created": "2025-04-16T11:00:00",
    "lastUpdated": "2025-04-16T13:00:00"
  }
]
```

---

### **2. Buscar linha por ID**
- **Rota:** `GET /api/Line/BuscarLinha/{id}`
- **Descrição:** Retorna uma linha específica pelo ID.
- **Autorização:** Não requer autenticação.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/Line/BuscarLinha/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "name": "Linha 1",
  "created": "2025-04-16T10:00:00",
  "lastUpdated": "2025-04-16T12:00:00"
}
```

---

### **3. Buscar linha por nome**
- **Rota:** `GET /api/Line/BuscarNome/{name}`
- **Descrição:** Retorna uma linha específica pelo nome.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/Line/BuscarNome/Linha%201 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "name": "Linha 1",
  "created": "2025-04-16T10:00:00",
  "lastUpdated": "2025-04-16T12:00:00"
}
```

---

### **4. Criar ou atualizar uma linha**
- **Rota:** `POST /api/Line/adicionarLinha`
- **Descrição:** Adiciona uma nova linha ou atualiza uma linha existente.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X POST http://localhost:5000/api/Line/adicionarLinha \
-H "Content-Type: application/json" \
-H "Authorization: Bearer <TOKEN>" \
-d '{
  "id": 1,
  "name": "Linha Atualizada"
}'
```
- **Exemplo de resposta (atualização):**
```json
{
  "id": 1,
  "name": "Linha Atualizada",
  "created": "2025-04-16T10:00:00",
  "lastUpdated": "2025-04-16T14:00:00"
}
```
- **Exemplo de resposta (adição):**
```json
{
  "id": 3,
  "name": "Nova Linha",
  "created": "2025-04-16T15:00:00",
  "lastUpdated": "2025-04-16T15:00:00"
}
```

---

### **5. Deletar uma linha**
- **Rota:** `DELETE /api/Line/DeleteLinha/{id}`
- **Descrição:** Deleta uma linha específica pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X DELETE http://localhost:5000/api/Line/DeleteLinha/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "name": "Linha 1"
}
```

---

## Observações
- Substitua `<TOKEN>` pelo token JWT obtido no endpoint de autenticação.
- Certifique-se de que o servidor está rodando na porta correta (exemplo: `http://localhost:5000`).
- Para endpoints protegidos, é necessário incluir o cabeçalho `Authorization` com o token JWT.
- O endpoint de adicionar ou atualizar (`POST /api/Line/adicionarLinha`) verifica se o ID já existe:
  - Se o ID existir, a linha será atualizada.
  - Se o ID não existir, uma nova linha será criada.

#### Station View
O endpoint de Estações View é responsável por:

Adicionar ou atualizar monitores no mapa de estações:

Garante que cada monitor ESD seja corretamente vinculado a uma estação e linha de produção.
Valida a combinação entre monitor, estação e linha para evitar duplicidades ou inconsistências.
Gerar um mapa consolidado de estações e monitores:

Fornece uma visão geral do sistema, mostrando as associações entre linhas, estações e monitores.
Manter a integridade do sistema:

Valida os dados antes de adicionar ou atualizar registros.
Remove associações inválidas ou desnecessárias ao deletar uma estação view.
### **1. Listar todas as estações view**
- **Rota:** `GET /api/StationView/todasEstacaoView`
- **Descrição:** Retorna todas as estações view cadastradas.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/StationView/todasEstacaoView \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
[
  {
    "id": 1,
    "monitorEsdId": 2,
    "linkStationAndLineId": 3,
    "positionSequence": 1,
    "monitorEsd": {
      "id": 2,
      "serialNumberEsp": "SN12345",
      "description": "Monitor 1"
    },
    "linkStationAndLine": {
      "id": 3,
      "lineID": 1,
      "stationID": 4
    }
  }
]
```

---

### **2. Buscar estação view por ID**
- **Rota:** `GET /api/StationView/BuscarEstacaoView/{id}`
- **Descrição:** Retorna uma estação view específica pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/StationView/BuscarEstacaoView/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "monitorEsdId": 2,
  "linkStationAndLineId": 3,
  "positionSequence": 1,
  "monitorEsd": {
    "id": 2,
    "serialNumberEsp": "SN12345",
    "description": "Monitor 1"
  },
  "linkStationAndLine": {
    "id": 3,
    "lineID": 1,
    "stationID": 4
  }
}
```

---

### **3. Criar ou atualizar uma estação view**
- **Rota:** `POST /api/StationView/adicionarEstacaoView`
- **Descrição:** Adiciona ou atualiza uma estação view.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X POST http://localhost:5000/api/StationView/adicionarEstacaoView \
-H "Content-Type: application/json" \
-H "Authorization: Bearer <TOKEN>" \
-d '{
  "id": 1,
  "monitorEsdId": 2,
  "linkStationAndLineId": 3,
  "positionSequence": 1
}'
```
- **Exemplo de resposta (atualização):**
```json
{
  "id": 1,
  "monitorEsdId": 2,
  "linkStationAndLineId": 3,
  "positionSequence": 1
}
```
- **Exemplo de resposta (adição):**
```json
{
  "id": 4,
  "monitorEsdId": 5,
  "linkStationAndLineId": 6,
  "positionSequence": 2
}
```

---

### **4. Gerar mapa de fábricas**
Explicação Adicional
Este endpoint é essencial para garantir que os monitores ESD sejam corretamente associados às estações e linhas de produção. Ele organiza o mapa de estações e monitores, permitindo uma visão consolidada e funcional do sistema. O endpoint também valida os dados fornecidos, garantindo a integridade e consistência das associações no banco de dados. ```
- **Rota:** `GET /api/StationView/factoryMap`
- **Descrição:** Gera um mapa consolidado das linhas, estações e monitores ESD.
- **Autorização:** Não requer autenticação.
- **Exemplo de requisição:**
```bash
curl -X GET http://localhost:5000/api/StationView/factoryMap
```
- **Exemplo de resposta:**
```json
[
  {
    "line": {
      "id": 1,
      "name": "Linha 1"
    },
    "stations": [
      {
        "station": {
          "id": 4,
          "name": "Estação 1"
        },
        "monitorsEsd": [
          {
            "monitorsEsd": {
              "id": 2,
              "serialNumberEsp": "SN12345",
              "description": "Monitor 1"
            },
            "positionSequence": 1
          }
        ]
      }
    ]
  }
]
```

---

### **5. Deletar uma estação view**
- **Rota:** `DELETE /api/StationView/deleteStationView/{id}`
- **Descrição:** Deleta uma estação view específica pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.
- **Exemplo de requisição:**
```bash
curl -X DELETE http://localhost:5000/api/StationView/deleteStationView/1 \
-H "Authorization: Bearer <TOKEN>"
```
- **Exemplo de resposta:**
```json
{
  "id": 1,
  "monitorEsdId": 2,
  "linkStationAndLineId": 3
}
```

---

## Observações
- Substitua `<TOKEN>` pelo token JWT obtido no endpoint de autenticação.
- Certifique-se de que o servidor está rodando na porta correta (exemplo: `http://localhost:5000`).
- Para endpoints protegidos, é necessário incluir o cabeçalho `Authorization` com o token JWT.
- O endpoint de criar ou atualizar (`POST /api/StationView/adicionarEstacaoView`) verifica se a combinação de `monitorEsdId` e `linkStationAndLineId` já existe:
  - Se existir, retorna um erro.
  - Caso contrário, cria ou atualiza a estação view.
- O endpoint de mapa de fábricas (`GET /api/StationView/factoryMap`) consolida dados de linhas, estações e monitores ESD para fornecer uma visão geral do sistema.
---

# API de Gerenciamento de Logs de Monitores ESD

Esta API permite gerenciar os logs de monitores ESD, incluindo operações como listar, buscar, criar, atualizar e deletar logs. Também oferece funcionalidades para alterar o status de logs e buscar logs por diferentes critérios, como tipo, conteúdo, IP e Serial Number.

---

## Endpoints Disponíveis

### **1. Listar logs de monitor ESD por ID**
- **Rota:** `GET /api/LogMonitorEsd/ListMonitorEsd`
- **Descrição:** Retorna uma lista de logs de monitor ESD por ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

### **2. Listar logs em ordem crescente**
- **Rota:** `GET /api/LogMonitorEsd/ListLogsOrdemCrescente`
- **Descrição:** Retorna logs de monitor ESD em ordem crescente.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

### **3. Listar logs em ordem decrescente**
- **Rota:** `GET /api/LogMonitorEsd/ListLogsOrdemDecrescente`
- **Descrição:** Retorna logs de monitor ESD em ordem decrescente.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

### **4. Buscar log por ID**
- **Rota:** `GET /api/LogMonitorEsd/LOGBYID/{id}`
- **Descrição:** Retorna um log específico pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

### **5. Buscar logs por tipo**
- **Rota:** `GET /api/LogMonitorEsd/TYPE/{type}`
- **Descrição:** Retorna logs de monitor ESD por tipo de mensagem.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

### **6. Buscar logs por conteúdo**
- **Rota:** `GET /api/LogMonitorEsd/CONTENT/{content}`
- **Descrição:** Retorna logs de monitor ESD por conteúdo da mensagem.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

### **7. Buscar monitor ESD por ID**
- **Rota:** `GET /api/LogMonitorEsd/ID/{id}`
- **Descrição:** Retorna informações de um monitor ESD específico pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

### **8. Buscar monitor ESD por IP**
- **Rota:** `GET /api/LogMonitorEsd/IP/{ip}`
- **Descrição:** Retorna informações de um monitor ESD específico pelo IP.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

### **9. Buscar monitor ESD por Serial Number**
- **Rota:** `GET /api/LogMonitorEsd/sn/{serialNumber}`
- **Descrição:** Retorna informações de um monitor ESD específico pelo Serial Number.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

### **10. Criar ou atualizar log**
- **Rota:** `POST /api/LogMonitorEsd/ManagerLogs`
- **Descrição:** Cria ou atualiza um log de monitor ESD.
- **Autorização:** Não requer autenticação.
- curl -X POST http://localhost:5000/api/LogMonitorEsd/ManagerLogs \
-H "Content-Type: application/json" \
-d '{
  "id": 1,
  "serialNumberEsp": "SN12345",
  "messageType": "operador",
  "messageContent": "Operador conectado",
  "status": 1,
  "description": "Log de teste"
}'

---

### **11. Alterar status de log**
- **Rota:** `POST /api/LogMonitorEsd/changeLogs`
- **Descrição:** Altera o status de um log de monitor ESD.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

### **12. Deletar log por ID**
- **Rota:** `DELETE /api/LogMonitorEsd/{id}`
- **Descrição:** Deleta um log de monitor ESD específico pelo ID.
- **Autorização:** Requer autenticação com as roles `administrador` ou `tecnico`.

---

## Observações
- Substitua `<TOKEN>` pelo token JWT obtido no endpoint de autenticação.
- Certifique-se de que o servidor está rodando na porta correta (exemplo: `http://localhost:5000`).
- Para endpoints protegidos, é necessário incluir o cabeçalho `Authorization` com o token JWT.
- O endpoint de criar ou atualizar (`POST /api/LogMonitorEsd/ManagerLogs`) verifica se o log já existe:
  - Se existir, atualiza os dados.
  - Caso contrário, cria um novo log.
## Como rodar o sistema

1. Execute o script `start.ps1` para iniciar o frontend:

```powershell
.\start.ps1
```

2. Use o script Python abaixo para descobrir o IP da rede local:

```python
import socket

def get_ip():
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    s.connect(('8.8.8.8', 80))
    print(s.getsockname()[0])
    s.close()

get_ip()
```

3. Adicione esse endereço IP no CORS do backend em C#:

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("CorsPolicy", builder =>
    {
        builder.WithOrigins("http://<SEU_IP>:3000")
               .AllowAnyHeader()
               .AllowAnyMethod();
    });
});
```

---

## Acesso ao sistema

- **Usuário:** admin  
- **Senha:** admcompal

---

## Testes E2E com Cypress

O projeto inclui testes de ponta-a-ponta utilizando o Cypress.

### Estrutura dos testes

- `cypress/e2e/` – Testes principais:
  - `factoryMap.cy.js`
  - `station.cy.js`
  - `monitor.cy.js`
- `cypress/support/` – Comandos personalizados e configurações globais
- `cypress/fixtures/` – Dados simulados utilizados nos testes

### Observações

- Os testes de **estação** e **monitor** requerem que exista uma **linha cadastrada**.
- O bloco `describe` dos testes já realiza a criação da linha automaticamente.
- Todos os testes executam login automaticamente antes de iniciar.

### Como rodar os testes

Com o sistema rodando, execute:

```bash
npx cypress open
```
