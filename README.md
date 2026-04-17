# 🤖 BOT de Conversação via Telegram — n8n

Workflow n8n para um assistente pessoal no Telegram capaz de receber mensagens de **texto ou áudio**, processar via IA (GPT-4o) e responder no mesmo formato da entrada.

---

## ✨ Funcionalidades

- Recebe mensagens de **texto** ou **áudio (voz)** pelo Telegram
- Detecta automaticamente o tipo de entrada e roteia o fluxo
- Transcreve áudios via **OpenAI Whisper**
- Processa a mensagem com um **AI Agent** (GPT-4o) com memória de contexto por sessão (janela de 10 mensagens)
- Responde com **texto** se a entrada foi texto, ou com **áudio MP3** se a entrada foi voz
- Exibe o indicador de "digitando..." enquanto processa

---

## 🔧 Tecnologias

| Serviço | Uso |
|---|---|
| **n8n** | Orquestração do workflow |
| **Telegram Bot API** | Trigger e envio de respostas |
| **OpenAI GPT-4o** | AI Agent para geração de respostas |
| **OpenAI Whisper** | Transcrição de áudio para texto |
| **OpenAI TTS** | Geração de áudio MP3 a partir do texto |

---

## 🗺️ Fluxo do Workflow

```
Telegram Trigger
    ├── Typing Action (indicador visual)
    └── Switch: Text or Voice?
            ├── [Texto]  → Send Context to AI Agent
            └── [Voz]    → Get Voice → Transcribe → Send Transcribe Context to AI Agent
                                                              ↓
                                                          AI Agent (GPT-4o + Memory)
                                                              ↓
                                                    Switch: origem é voz ou texto?
                                                        ├── [Voz]   → Generate Audio → Send Audio
                                                        └── [Texto] → Send Text Response
```

---

## ⚙️ Pré-requisitos

- Instância **n8n** (self-hosted ou cloud)
- **Bot do Telegram** criado via [@BotFather](https://t.me/botfather)
- Conta na **OpenAI** com acesso à API

---

## 🚀 Como usar

1. Importe o arquivo `.json` no n8n via **Settings > Import Workflow**
2. Configure as credenciais:
   - `Telegram API` → Token do seu bot
   - `OpenAI API` → Sua chave de API
3. Ative o workflow
4. Envie uma mensagem de texto ou um áudio para o bot no Telegram

---

## 📝 Observações

- O `sessionId` é baseado no `chat.id` do Telegram, garantindo memória separada por usuário
- A janela de memória é de **10 mensagens** por sessão
- O áudio de resposta é gerado no formato **MP3**
- O system prompt pode ser customizado diretamente no nó **AI Agent**
