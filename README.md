
```markdown
# Projeto com ESP32 e Servidor Web para Controle de LED

Este código permite o controle de um LED conectado ao ESP32 através de uma página web. O projeto utiliza a biblioteca WiFi para conectar o ESP32 a uma rede Wi-Fi e a biblioteca WebServer para criar uma interface web onde é possível ligar ou desligar o LED remotamente.

---

## Bibliotecas Utilizadas

```cpp
#include <WiFi.h>        // Biblioteca para gerenciar a conexão Wi-Fi
#include <WebServer.h>    // Biblioteca para criar o servidor web
```

Essas bibliotecas são essenciais para gerenciar a conexão Wi-Fi e criar o servidor web que permitirá a interação via navegador.

---

## Configurações de Rede

```cpp
const char* ssid = "SSID";
const char* password = "PASSWORD";
```

Aqui, você deve substituir `"SSID"` pelo nome da sua rede Wi-Fi e `"PASSWORD"` pela senha da sua rede.

---

## Instância do Servidor e Controle do LED

```cpp
WebServer server(80);  // Cria uma instância do servidor web na porta 80
const int ledPin = 4;  // Define o pino do LED
bool ledState = LOW;   // Estado inicial do LED (desligado)
```

- **server(80):** Configura o servidor web na porta 80, a porta padrão para HTTP.
- **ledPin:** Define o pino ao qual o LED está conectado.
- **ledState:** Variável que armazena o estado do LED (ligado ou desligado).

---

## Função `handleRoot()`

Essa função gera a página web com dois botões: um para ligar e outro para desligar o LED.

```cpp
void handleRoot() {
  String html = "<!DOCTYPE html>";
  html += "<html lang='en'>";
  ...
  html += "</html>";
  server.send(200, "text/html", html);
}
```

- **HTML Dinâmico:** A página web é construída dinamicamente com HTML e CSS, oferecendo um design estiloso e simples para o controle do LED.
- **Botões para Controle:** A página tem botões para ligar e desligar o LED, utilizando estilos diferenciados para melhorar a experiência do usuário.

---

## Funções para Ligar e Desligar o LED

### Ligar o LED:

```cpp
void handleLedOn() {
  ledState = HIGH;
  digitalWrite(ledPin, ledState);
  server.sendHeader("Location", "/");
  server.send(303);
}
```

- **ledState = HIGH:** Atualiza o estado do LED para ligado.
- **digitalWrite:** Liga o LED no pino definido.
- **Redirecionamento:** Após ligar o LED, o navegador é redirecionado de volta para a página principal.

### Desligar o LED:

```cpp
void handleLedOff() {
  ledState = LOW;
  digitalWrite(ledPin, ledState);
  server.sendHeader("Location", "/");
  server.send(303);
}
```

Semelhante à função para ligar, mas aqui o LED é desligado.

---

## Função `setup()`

```cpp
void setup() {
  Serial.begin(115200);
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, ledState);
  
  WiFi.begin(ssid, password);
  ...
  server.begin();
}
```

- **Conexão Wi-Fi:** O ESP32 se conecta à rede Wi-Fi usando o SSID e a senha fornecidos.
- **Configuração do LED:** O pino do LED é configurado como saída, garantindo que o LED comece desligado.
- **Rotas do Servidor:** O servidor web é configurado para responder a três rotas: `/` (página principal), `/on` (liga o LED), e `/off` (desliga o LED).

---

## Função `loop()`

```cpp
void loop() {
  server.handleClient();
}
```

Esta função é chamada repetidamente e mantém o servidor web em execução, respondendo às requisições do navegador.

---

## Estilos da Página Web

O design da página web foi criado para ser simples e funcional. Aqui estão alguns detalhes importantes do CSS:

- **Plano de fundo escuro** para reduzir o brilho da página.
- **Botões estilizados** com diferentes cores: verde para ligar e vermelho para desligar, com efeitos de sombra ao passar o mouse.
- **Caixa centralizada** para o conteúdo da página, melhorando a visualização e a experiência do usuário.

---

## Funcionamento Geral

- Acesse o endereço IP do ESP32 no navegador (mostrado no Serial Monitor após a conexão Wi-Fi).
- Utilize os botões na interface para ligar ou desligar o LED.
- O estado atual do LED (ligado/desligado) será exibido na página.

---

## Conclusão

Este projeto demonstra como criar uma interface web simples e interativa para controlar dispositivos conectados ao ESP32, permitindo o controle de um LED através de uma rede Wi-Fi. É uma excelente base para explorar mais funcionalidades, como sensores ou outros atuadores.
```

Esse markdown oferece uma explicação detalhada do código e divide-o em seções lógicas, facilitando a leitura e entendimento.
