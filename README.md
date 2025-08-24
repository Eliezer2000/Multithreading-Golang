# Desafio Go: Consulta de CEP com APIs Concorrentes 🚀

Bem-vindo ao **Desafio Go: Consulta de CEP com APIs Concorrentes**, um projeto do meu portfólio que demonstra o uso de concorrência em Go para consultar duas APIs de CEP (BrasilAPI e ViaCEP) simultaneamente, retornando a resposta mais rápida. O projeto utiliza goroutines, channels e context para gerenciar consultas concorrentes com um timeout de 1 segundo.

## 📌 Sobre o Desafio

Este desafio tem como objetivo aplicar conceitos avançados de concorrência em Go, implementando um sistema que:

- **Consulta duas APIs de CEP simultaneamente:**
  - BrasilAPI
  - ViaCEP
- **Retorna a resposta da API mais rápida**
- **Descarta a resposta mais lenta**
- **Exibe no terminal os dados do endereço e a API que respondeu**
- **Limita o tempo de resposta em 1 segundo (timeout)**
- **Exibe mensagens de erro claras em caso de timeout ou outros problemas**

## 📋 Requisitos do Desafio

- [x] Consultar simultaneamente as APIs BrasilAPI e ViaCEP
- [x] Retornar a resposta mais rápida e descartar a mais lenta
- [x] Exibir no terminal o endereço completo (CEP, rua, bairro, cidade, estado) e a API que respondeu
- [x] Aplicar um timeout global de 1 segundo usando context
- [x] Tratar erros (timeout, CEP não encontrado, etc.) com mensagens claras
- [x] Unificar os formatos de resposta das APIs em uma estrutura comum

## 📦 Estrutura do Projeto

```
consulta-cep-concorrente/
├── main.go                 # Código principal
├── README.md               # Documentação do projeto
└── go.mod                  # Arquivo de dependências Go
```

## 🚀 Como Executar

### Pré-requisitos

- Go (versão 1.22 ou superior) instalado
- Acesso à internet para consumir as APIs BrasilAPI e ViaCEP

### Passos

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/<seu-usuario>/consulta-cep-concorrente.git
   cd consulta-cep-concorrente
   ```

2. **Execute o programa:**
   ```bash
   # Passe um CEP como argumento
   go run main.go 01153000
   ```

3. **Verifique a saída:**
   - O programa exibe o endereço retornado pela API mais rápida ou uma mensagem de erro (timeout ou CEP inválido)

## 📊 Exemplo de Saída

### Caso de sucesso:
```
API: BrasilAPI
CEP: 01153000
Rua: Rua Vitorino Carmilo
Bairro: Barra Funda
Cidade: São Paulo
Estado: SP
```

### Caso de erro (CEP não encontrado):
```
Erro da API ViaCEP: CEP não encontrado
```

### Caso de timeout:
```
Timeout: nenhuma resposta recebida em 1 segundo
```

## 🛠 Tecnologias Utilizadas

- **🐹 Go 1.22+**: Linguagem principal
- **🌐 BrasilAPI**: API de CEP brasileira (JSON com campos em inglês)
- **🌐 ViaCEP**: API de CEP alternativa (JSON com campos em português)
- **⏱️ Context**: Controle de timeout global (1 segundo)
- **⚡ Goroutines**: Execução concorrente das consultas às APIs
- **📡 Channels**: Comunicação entre goroutines para selecionar a resposta mais rápida

## 🔧 Funcionamento do Código

### Estruturas de Dados

```go
type Adress struct {
    CEP          string `json:"cep"`
    Street       string `json:"street"`
    Neighborhood string `json:"neighborhood"`
    City         string `json:"city"`
    State        string `json:"state"`
    API          string `json:"-"`
}

type CepResult struct {
    API   string
    Data  *Adress
    Error error
}
```

### Fluxo do Programa

1. **Valida o argumento CEP** (8 dígitos numéricos)
2. **Cria um contexto com timeout de 1 segundo** usando `context.WithTimeout`
3. **Inicia duas goroutines** para consultar as APIs BrasilAPI e ViaCEP simultaneamente
4. **Usa um canal bufferizado** para receber os resultados das consultas
5. **Seleciona a primeira resposta válida** recebida pelo canal
6. **Cancela a consulta pendente** (mais lenta) usando o contexto
7. **Exibe o resultado no terminal** (endereço ou erro)

### APIs Utilizadas

- **BrasilAPI**: Retorna dados em JSON com campos em inglês (ex.: street, neighborhood)
- **ViaCEP**: Retorna dados em JSON com campos em português (ex.: logradouro, bairro)

### Principais Características

- **Concorrência eficiente**: Usa goroutines para consultas paralelas e channels para sincronização
- **Timeout controlado**: Limite de 1 segundo para ambas as APIs
- **Tratamento de erros**: Mensagens específicas para timeout, CEP não encontrado ou erros de rede
- **Unificação de dados**: Converte respostas das APIs para uma estrutura comum (Adress)

## 📝 Observações

- O programa prioriza a primeira resposta válida recebida
- Se uma API falhar, a outra ainda pode retornar sucesso
- O timeout de 1 segundo é aplicado globalmente às duas consultas
- A estrutura `Adress` unifica os formatos de resposta das APIs para facilitar o uso

## 📜 Licença

Este projeto está licenciado sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para detalhes.


## 👨‍💻 Autor

Desenvolvido por **Eliezer Alves** como parte de um desafio de programação em Go.
