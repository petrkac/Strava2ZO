# Strava2ZO
Node-RED flow for integrating Strava into the Live Dashboard

Nejdříve si připrav Strava aplikaci. Postup je zde: https://developers.strava.com/docs/getting-started/

Do Node RED si naimportuj soubor.

Do "Set Constants" doplň proměnné:

Strava API přihlašovací údaje, podle skutečných údajů dle https://developers.strava.com/docs/getting-started/
flow.set("stravaAuth", {
    client_id: "XXXXXX",
    client_secret: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    refresh_token: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    grant_type: "refresh_token"
});


flow.set("importKey", "XXXXXXXXXXXXXXX"); // Import key pro živý obraz
flow.set("clubId", "XXXXXXXX"); // ID klubu na Strava
flow.set("maxRunners", 9); // Maximální počet běžců odesílaných na živý obraz


// Strava nevrací datum aktivity. Vrací všechny aktivity (pokud je klub nastaven na Multisport) všech v aktuálních účastníků klubu 

Nastavení poslední aktivity - ta už se nepočítá
flow.set("lastActivity", {
    name: "Afternoon Run",
    distance: 21326.8
});

Aktivity jsou ze strava API načítány a zpracovávány dokud se nedojde na konec seznamu, nebo nenarazí na poslední aktivitu.

Jsou počítány jen aktivity Walk a Run. Je možno upravit v Process Activities
    // Přičítáme pouze platné aktivity
    if (!stopCounting && (activity.sport_type === "Run" || activity.sport_type === "Walk")) {
        runners[key].distance += activity.distance;
    }



