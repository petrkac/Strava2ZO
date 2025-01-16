# Strava2ZO
Node-RED flow for integrating Strava into the Live Dashboard (Živý Obraz).

---

## 🌟 Prerequisites
1. **Prepare your Strava application:**  
   Follow the instructions here: [Strava API Getting Started](https://developers.strava.com/docs/getting-started/).
2. **Import the Node-RED JSON file:**  
   Use the provided JSON file to set up the flow in Node-RED.
3. **Prepare your Živý obraz ID - https://zivyobraz.eu/?page=muj-ucet&hodnoty=1

---

## 🛠️ Configuration
### Set Constants
Open the `Set Constants` node and configure the following variables:

```javascript
// Strava API credentials
flow.set("stravaAuth", {
    client_id: "XXXXXX",
    client_secret: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    refresh_token: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    grant_type: "refresh_token"
});

// Import key for Live Dashboard
flow.set("importKey", "XXXXXXXXXXXXXXX");

// Strava club ID
flow.set("clubId", "XXXXXXXX");

// Maximum number of runners sent to Live Dashboard
flow.set("maxRunners", 9);
```


## Last Activity
To prevent duplicate calculations, set the last processed activity in the Set Constants node:

```javascript
flow.set("lastActivity", {
    name: "Afternoon Run",
    distance: 21326.8
});
```


## 📋 How It Works
Activity Fetching: The flow fetches activities from the Strava API.
Processing Activities: Activities are processed until either the end of the list is reached or the last processed activity is encountered.
Supported Activity Types:
Only Run and Walk activities are counted. To modify this, update the condition in the Process Activities node:

```javascript
if (!stopCounting && (activity.sport_type === "Run" || activity.sport_type === "Walk")) {
    runners[key].distance += activity.distance;
}
```

## 🔗 Additional Notes
Strava API Behavior:
The Strava API does not return the activity date, but it fetches all activities from the current club members.
Ensure your club is set to Multisport if multiple activity types are needed.
## 🧰 Troubleshooting
Activities are not being fetched:
Verify your Strava API credentials in the Set Constants node.
Incorrect or unexpected data:
Check the lastActivity configuration to ensure the correct activity is excluded.

