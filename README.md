# Laboratorio_RPC

**Disciplina:** Programação Paralela e Distribuída – 2025/2  
**Professor:** Breno Krohling  
**Linguagem:** Python 3  
**Tecnologia:** gRPC (Google Remote Procedure Call)
**Integrantes do grupo:** Daniely de Sá Cadette, Lívya Victoria Pereira Ferreira, Lorraine Efigênio dos Santos, Mayara Modesto Bregonci



## Descrição do Projeto

Este projeto implementa **duas aplicações distribuídas** independentes utilizando o modelo de **Chamada de Procedimento Remoto (RPC)** com o framework **gRPC**.  
Cada aplicação possui seu próprio **servidor** e **cliente**, definidos através de arquivos `.proto` e comunicação baseada em **Protocol Buffers (Protobuf)**.

### Aplicações desenvolvidas:
1. **Calculadora RPC** – Executa operações aritméticas básicas (soma, subtração, multiplicação e divisão) entre cliente e servidor.  
2. **Minerador RPC** – Simula a mineração de criptomoedas com desafios criptográficos e múltiplos clientes competindo por soluções válidas.



##  Estrutura de Pastas

```
Laboratorio_RPC/
│
├── calculator.proto
├── calculator_server.py
├── calculator_client.py
│
├── miner.proto
├── miner_server.py
├── miner_client.py
│
└── Laboratorio_RPC_Servidores_Separados.ipynb   # Notebook Colab pronto para execução
```



## Tecnologias Utilizadas

- **Python 3**
- **gRPC** – Comunicação remota eficiente
- **Protocol Buffers (Protobuf)** – Serialização binária leve
- **Threading** – Para mineração paralela
- **Google Colab** – Ambiente de execução



## Instalação

### 1️ Instalar dependências
No terminal local ou célula Colab:
```bash
pip install grpcio grpcio-tools
```

### 2️ Compilar os arquivos `.proto`
Execute no terminal ou Colab:
```bash
python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. calculator.proto
python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. miner.proto
```

Isso gera automaticamente os arquivos:
```
calculator_pb2.py
calculator_pb2_grpc.py
miner_pb2.py
miner_pb2_grpc.py
```



## Parte 1 – Calculadora RPC

### Servidor (`calculator_server.py`)
- Expõe quatro métodos RPC: `Add`, `Sub`, `Mul`, `Div`.
- Processa as operações e retorna os resultados para o cliente.
- Executa na porta **50051**.

### Cliente (`calculator_client.py`)
- Exibe menu interativo no terminal.
- Envia chamadas RPC para o servidor e exibe o resultado.

#### Execução:
```bash
# Iniciar servidor
python calculator_server.py
# Em outro terminal 
python calculator_client.py
```



## Parte 2 – Minerador RPC

### Servidor (`miner_server.py`)
- Mantém tabela de transações (`TransactionID`, `Challenge`, `Solution`, `Winner`).
- Gera novos desafios automaticamente.
- Valida soluções enviadas pelos clientes.
- Executa na porta **50052**.

### Cliente (`miner_client.py`)
- Oferece menu com funções de consulta e mineração.
- A mineração usa **threads paralelas** para acelerar o processo de busca de soluções SHA-1 válidas.

#### Execução:
```bash
# Iniciar servidor
python miner_server.py
# Em outro terminal 
python miner_client.py
```

No menu do cliente:
1. Use `1) getTransactionID` para obter o ID da transação atual.  
2. Use `2) getChallenge` para visualizar o desafio.  
3. Use `6) Mine` para iniciar a mineração.  
4. O primeiro cliente que enviar uma solução válida vence a rodada.



## Observações Importantes

- **Execute um servidor por vez** (Calculadora **ou** Minerador).  
- Use **portas diferentes** para cada serviço (`50051` e `50052`).  
- No Colab, servidores devem ser executados com `nohup` ou em background.  
- Recompile os `.proto` se modificar a interface RPC.  
- A dificuldade da mineração varia automaticamente entre **1 e 6 zeros no hash**.  



## Testes Realizados

- Testes funcionais executados em ambiente **Google Colab** e local.  
- Comunicação cliente/servidor estável e de baixa latência.  
- Validação de erros implementada (como divisão por zero e IDs inválidos).  
- Múltiplas threads no minerador mostraram aumento significativo na taxa de sucesso.  



## Resultados Obtidos

| Aplicação | Resultado | Observações |
|------------|------------|-------------|
| **Calculadora RPC** | Comunicação e operações corretas via gRPC | Latência muito baixa e fácil extensão de métodos |
| **Minerador RPC** | Simulação bem-sucedida de prova de trabalho | Controle de concorrência e criação automática de novos desafios |



## Conclusões

O projeto demonstrou com sucesso o uso de **RPC em sistemas distribuídos**, mostrando:
- comunicação eficiente entre cliente e servidor;  
- isolamento de lógica e interfaces via `.proto`;  
- uso de **threads** para paralelismo;  
- robustez e escalabilidade do gRPC.

Essas implementações consolidam os conceitos teóricos de **programação paralela e distribuída**, unindo prática e teoria de forma aplicada.


 
