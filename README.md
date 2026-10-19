# NOVA AI

Assistente de voz em português com rosto animado (React + Vite + Express + Groq + Edge TTS). Instalável como PWA no Android.

## Requisitos
Node.js 20+ e uma conta gratuita na Groq.

## Instalação
```bash
npm install
cp .env.example .env     # depois edite o .env
```
**Chave Groq:** crie em https://console.groq.com/keys e cole em `GROQ_API_KEY` no `.env`. Nunca envie a chave em chats nem a publique no Git (o `.env` já está no `.gitignore`).

Modelos (trocáveis no `.env`): chat `llama-3.3-70b-versatile`, transcrição `whisper-large-v3-turbo` — ambos listados como modelos de produção na documentação da Groq em 18/10/2026. Confira em `GET https://api.groq.com/openai/v1/models`.

## Executar
```bash
npm run dev      # API (3001) + app (http://localhost:5173)
```
Produção local: `npm run build && npm start` (o Express serve `dist/` e a API na mesma porta).

## Testar
- **Texto:** digite no campo e envie; a Nova responde e fala.
- **Voz:** toque no microfone, fale, toque de novo para enviar. O navegador exige HTTPS (ou localhost) para liberar o microfone. No celular em rede local, use um túnel HTTPS (ex.: `cloudflared tunnel --url http://localhost:5173`).
- **Automáticos:** `npm test` (servidor), `npm run build` e `npm run check:bundle` (confere que não há chave no bundle).

## Publicar (Render, plano gratuito)
1. Suba o projeto em um repositório **privado** no GitHub.
2. Render → New → Web Service. Build: `npm install && npm run build`. Start: `npm start`.
3. Em Environment, defina `GROQ_API_KEY` e, recomendado, `ACCESS_CODE` (o app pedirá o código; protege sua cota).
4. Frontend e API ficam no mesmo domínio HTTPS, sem CORS extra.
Obs.: o plano gratuito "dorme" após inatividade; a primeira resposta pode demorar.

## Instalar no Android
Abra a URL pública no Chrome → menu ⋮ → **Instalar app** / **Adicionar à tela inicial**.

## Limitações conhecidas
- Fluxo em etapas (gravar → transcrever → responder → falar), não é tempo real.
- Edge TTS usa um serviço não oficial da Microsoft; pode falhar em alguns hosts. Nesse caso o app usa a voz do navegador (pt-BR se existir; senão a voz padrão do aparelho).
- O app só funciona com internet; o cache guarda apenas a interface, nunca conversas ou áudio.
- O histórico fica na sessão do navegador (sessionStorage); não há memória permanente.
- Limite de 40 requisições/min por IP e áudio de até 60 s / 10 MB.
