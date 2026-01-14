# LærlingBot

En intelligent chatbot som hjelper lærlinger og ansatte innen IT-utvikling med spørsmål relatert til kompetanseboka for VG3 IT-utvikler.

## Om prosjektet

LærlingBot er bygget med **OpenAI Agent Builder** og **ChatKit** for å gi lærlinger en enkel måte å få svar på spørsmål om læreplanen og kompetansekravene. Chatboten er koblet opp mot datasett fra kompetanseboka for VG3 løpet til IT-utvikler.

## Teknologi

- **Frontend**: SvelteKit
- **Chatbot**: OpenAI ChatKit
- **Backend**: SvelteKit API-ruter
- **Datasett**: Kompetanseboka VG3 IT-utvikler

## Installering

```sh
npm install
```

## Utvikling

Start utviklingsserveren:

```sh
npm run dev
```

## Miljøvariabler

Opprett en `.env` fil med følgende variabler:

```
OPENAI_API_KEY_LABS=din_openai_api_nøkkel
AGENT_WORKFLOW=din_workflow_id
```

## Bygging

```sh
npm run build
```
