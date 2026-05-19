# Evidências - INSERT, UPDATE e DELETE no banco db_em 
## Identificação 
Nome:Gabriely Vieira de Souza
Turma:3°Ano Ensino Médio B
Data:19/05/2026
--- 
# 1. SELECT final - Leituras do dia 2026-04-04 
Execute no DBeaver:
 
```sql 
SELECT * 
FROM leituras 
WHERE timestamp >= '2026-04-04'  AND timestamp < '2026-04-05' 
ORDER BY timestamp ASC; 
``` 
Cole abaixo a saída obtida: 
```text 
cole aqui a saída do SELECT 
``` 
id	station_id	timestamp	temperature_c	humidity_pct
9	EM-ARACATUBA-01	2026-04-04 08:00:00.000 -0300	23.4	76.2
10	EM-ARACATUBA-01	2026-04-04 09:00:00.000 -0300	25.2	72
11	EM-ARACATUBA-01	2026-04-04 10:00:00.000 -0300	25.9	70.5
--- 
# 2. SELECT final - Conferência do UPDATE Execute no DBeaver: 
```sql 
SELECT * 
FROM leituras 
WHERE station_id = 'EM-ARACATUBA-01'  AND timestamp = '2026-04-04 09:00:00'; ``` 
Cole abaixo a saída obtida: 
```text 
cole aqui a saída do SELECT 
``` 
id	station_id	timestamp	temperature_c	humidity_pct
10	EM-ARACATUBA-01	2026-04-04 09:00:00.000 -0300	25.2	72
--- 
# 3. SELECT final - Conferência do DELETE Execute no DBeaver: 
```sql 
SELECT * 
FROM leituras 
WHERE station_id = 'EM-ARACATUBA-01'  AND timestamp = '2026-04-04 11:00:00'; ``` 
Cole abaixo a saída obtida: 
```text

cole aqui a saída do SELECT 
``` 
id	station_id	timestamp	temperature_c	humidity_pct

Se o DELETE foi feito corretamente, esse SELECT não deverá retornar registros. --- 
# 4. SELECT final - Todas as leituras ordenadas 
Execute no DBeaver: 
```sql 
SELECT * 
FROM leituras 
ORDER BY id ASC; 
``` 
Cole abaixo a saída obtida: 
```text 
cole aqui a saída do SELECT 
``` 
id	station_id	timestamp	temperature_c	humidity_pct
1	EM-ARACATUBA-01	2026-04-01 08:00:00.000 -0300	24.5	72.1
2	EM-ARACATUBA-01	2026-04-01 09:00:00.000 -0300	25.8	69.4
3	EM-ARACATUBA-01	2026-04-01 10:00:00.000 -0300	27.2	65.8
4	EM-ARACATUBA-01	2026-04-02 08:00:00.000 -0300	23.8	73.5
5	EM-ARACATUBA-01	2026-04-02 09:00:00.000 -0300	24.6	71.2
6	EM-ARACATUBA-01	2026-04-02 10:00:00.000 -0300	25.4	68.9
7	EM-ARACATUBA-01	2026-04-02 11:00:00.000 -0300	26.1	66.4
8	EM-ARACATUBA-01	2026-04-02 12:00:00.000 -0300	27	63.8
9	EM-ARACATUBA-01	2026-04-04 08:00:00.000 -0300	23.4	76.2
10	EM-ARACATUBA-01	2026-04-04 09:00:00.000 -0300	25.2	72
11	EM-ARACATUBA-01	2026-04-04 10:00:00.000 -0300	25.9	70.5
--- 
# 5. Teste pela API 
Acesse no navegador: 
```text 
http://localhost:3000/api/leituras/data/2026-04-04 
``` 
Cole abaixo o resultado retornado pela API: 
```json 
require('dotenv').config();

const express = require('express');
const cors = require('cors');

const ensureDatabaseExists = require('./config/ensureDatabase');
const sequelize = require('./config/database');
const seedLeiturasIfEmpty = require('./seeders/seedLeiturasIfEmpty');
const leiturasRoutes = require('./routes/leiturasRoutes');

const app = express();

app.use(cors());
app.use(express.json());

app.get('/', (req, res) => { 
 return res.json({ 
 mensagem: 'API Estação Meteorológica', 
 descricao: 'API para consulta de leituras meteorológicas armazenadas no PostgreSQL.',  rotasDisponiveis: { 
 listarTodasAsLeituras: 'GET /api/leituras', 
 pesquisarLeiturasPorData: 'GET /api/leituras/data/2026-04-01', 
 importarCsv: 'npm run import:csv' 
 },
 formatoDaData: 'YYYY-MM-DD', 
 exemploDeUso: 'http://localhost:3000/api/leituras/data/2026-04-01', 
 }); 
}); 


app.use('/api', leiturasRoutes);

const PORT = process.env.PORT || 3000;

async function startServer() {
  try {
    await ensureDatabaseExists();

    await sequelize.authenticate();
    console.log('Conexão com PostgreSQL realizada com sucesso.');

    await sequelize.sync();
    console.log('Tabela sincronizada com sucesso.');

    await seedLeiturasIfEmpty();

    app.listen(PORT, () => {
      console.log(`Servidor rodando em http://localhost:${PORT}`);
    });
  } catch (error) {
    console.error('Erro ao iniciar a aplicação:');
    console.error(error.message);
    process.exit(1);
  }
}

startServer();

cole aqui o resultado da API 
``` 
{
  "dataPesquisada": "2026-04-04",
  "total": 3,
  "leituras": [
    {
      "id": 9,
      "station_id": "EM-ARACATUBA-01",
      "timestamp": "2026-04-04T11:00:00.000Z",
      "temperature_c": 23.4,
      "humidity_pct": 76.2
    },
    {
      "id": 10,
      "station_id": "EM-ARACATUBA-01",
      "timestamp": "2026-04-04T12:00:00.000Z",
      "temperature_c": 25.2,
      "humidity_pct": 72
    },
    {
      "id": 11,
      "station_id": "EM-ARACATUBA-01",
      "timestamp": "2026-04-04T13:00:00.000Z",
      "temperature_c": 25.9,
      "humidity_pct": 70.5
    }
  ]
}
--- 
# 6. Conclusão 
Explique com suas palavras a diferença entre INSERT, UPDATE e DELETE. Resposta: 
```text 
cole aqui sua resposta

INSERT: adiciona novos dados no banco de dados.
Ex: cadastrar um novo usuário.
UPDATE: altera dados que já existem.
Ex: mudar o nome de um usuário.
DELETE: apaga dados do banco de dados.
Ex: excluir um usuário cadastrado.


