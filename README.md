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

### 1. WeatherForecastController
- **Rota:** `GET /WeatherForecast`
- **Descrição:** Retorna uma previsão do tempo fictícia.

### 2. AuthenticationController
- **Rota:** `POST /api/Authentication`
- **Descrição:** Realiza login e retorna um token JWT.

### 3. UserController
- **Rota:** `GET /api/User`
- **Descrição:** Retorna todos os usuários cadastrados.

### 4. StationController
- **Rota:** `GET /api/Station/todosEstacoes`
- **Descrição:** Retorna todas as estações cadastradas.

### 5. LogMonitorEsdController
- **Rota:** `GET /api/LogMonitorEsd/ListMonitorEsd`
- **Descrição:** Retorna uma lista de logs de monitores ESD.

### 6. HubController
- **Rota:** `POST /api/Hub/send-log`
- **Descrição:** Envia um log para o Hub SignalR.

### 7. Outros Endpoints

#### StationViewController
- **Rota:** `GET /api/StationView/todasEstacaoView`

#### RolesController
- **Rota:** `GET /api/Roles/all`

#### ProduceActivityController
- **Rota:** `GET /api/ProduceActivity/TodaProducao`

#### MonitorEsdController
- **Rota:** `GET /api/MonitorEsd/todosmonitores`

#### LineController
- **Rota:** `GET /api/Line/TodasLinhas`

#### JigController
- **Rota:** `GET /api/Jig/todosJigs`

---

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
