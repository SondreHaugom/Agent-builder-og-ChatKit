<!--
  Orgbott Chatbot-komponent

  Bruker OpenAI ChatKit - et ferdigbygd chat-widget som gir fullstendig chatopplevelse.
  ChatKit håndterer UI, meldingshåndtering og kommunikasjon med OpenAI API.
-->
<svelte:head>
  <!-- Last inn ChatKit-bibliotek fra OpenAI CDN -->
  <script src="https://cdn.platform.openai.com/deployments/chatkit/chatkit.js" async></script>
</svelte:head>

<script>
  import { onMount } from 'svelte';

  /**
   * Henter client_secret fra backend for ChatKit-autentisering
   * ChatKit kaller denne automatisk når den trenger en ny session
   * @returns {Promise<string>} Client secret for ChatKit-session
   */
  async function getClientSecret() {
    try {
      const res = await fetch('/api/orgbotter', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ deviceId: 'test-device-123' })
      });
      const data = await res.json();
      console.log('Session created:', data);
      return data.client_secret;
    } catch (error) {

      console.error('Failed to get client secret:', error);
      throw error;
    }
  }

  

  onMount(() => {
    /**
     * Initialiserer ChatKit-widgeten når komponenten er montert
     * Bruker polling (setTimeout) for å vente på at ChatKit-skriptet lastes
     */
    const initChatKit = () => {
      const chatkit = /** @type {any} */ (document.getElementById('my-chat'));

      // Sjekk om ChatKit er lastet og klar
      if (chatkit && chatkit.setOptions) {
        console.log('ChatKit element found, initializing...');

        // Konfigurer ChatKit med getClientSecret-funksjon og tema
        const options = {
          api: {
            getClientSecret
          },
          theme: {
            colorScheme: "dark",
            color: {
              accent: {
                primary: "#eaeaea",
                level: 2
              }
            },
            radius: "round",
            density: "compact",
            typography: { fontFamily: "'Inter', sans-serif" },
          },
        };
        chatkit.setOptions(options);

        console.log('ChatKit options set successfully', options);
      } else {
        // ChatKit ikke lastet ennå, prøv igjen om 100ms
        console.log('Waiting for ChatKit to load...');
        setTimeout(initChatKit, 100);
      }
    };

    initChatKit();
  });


  
</script>


<div class="chat-container">
  <openai-chatkit id="my-chat"></openai-chatkit >
</div>

<style>
/* ---------- GLOBAL (html/body) ----------------------------------- */
:global(html), :global(body) {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  overflow: hidden;
}

/* ---------- FULL‑SCREEN CONTAINER ------------------------------- */
.chat-container {
  position: fixed;
  inset: 0;
  background: linear-gradient(
                135deg,
                #000000 0%,
                #0a0a0a 45%,
                #111111 100%
              );
  z-index: 9999;
}

/* ---------- MAKE CHATKIT TRANSPARENT ---------------------------- */
/* cspell:disable-next-line */
:global(openai-chatkit) {
  display: block !important;
  width: 100% !important;
  height: 100% !important;
  position: absolute;
  inset: 0;
  border-radius: 0 !important;
  background: transparent !important;           /* <‑‑ viktig */
}

/* Transparente barn (shadow‑DOM‑fallback) */
/* cspell:disable-next-line */
:global(openai-chatkit *),
/* cspell:disable-next-line */
:global(openai-chatkit ::part(*)),
/* cspell:disable-next-line */
:global(openai-chatkit ::slotted(*)) {
  background: transparent !important;
  /* cspell:disable-next-line */
  color: var(--chatkit-text-color, #eaeaea) !important;
}

/* ---------- CHATKIT THEME‑VARIABLES --------------------------- */
/* cspell:disable-next-line */
:global(:root) {
  /* cspell:disable-next-line */
  --chatkit-background-color: transparent !important;
  /* cspell:disable-next-line */
  --chatkit-primary-color:    #222222 !important;
  /* cspell:disable-next-line */
  --chatkit-text-color:       #eaeaea !important;
  /* cspell:disable-next-line */
  --chatkit-input-background:#111111 !important;
  /* cspell:disable-next-line */
  --chatkit-border-color:     #333333 !important;
}

/* ---------- OPTIONAL: MOBILE HEIGHT --------------------------- */
@media (max-width: 768px) {
  .chat-container { height: 80vh; }
}

</style>