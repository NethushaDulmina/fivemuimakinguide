
Hello! This is a simple guide to help developers who already know how to create UIs using **HTML**, **CSS**, **JavaScript**, or modern frameworks like **Vue.js** or **React.js**, but are unsure how to interact with FiveM. This guide focuses solely on the UI part, demonstrating how to send data to and retrieve data from FiveM scripts.

## **Understanding FiveM UI Interaction**

FiveM uses **NUI** to enable communication between your UI and the game. This communication happens through **events** and **messages**. Here's how to:

1. Send data from FiveM (Lua) to the UI.
2. Retrieve data from the UI and send it back to FiveM.

## **1. Sending Data to the UI**
### Example: Displaying Vehicle Details

**Lua Script: Sending Data**

```lua
-- Lua Script (FiveM)
RegisterCommand('openui', function()
    -- Send a message to the UI
    SendNUIMessage({
        type = "openmenu", -- A unique identifier for the message
        vehiclename = "Bmw M5" -- The message content
        vehicleclass = 8, -- sample number
        vehicleplate = 'ChillGuy'
    })
end)

```

**HTML**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FiveM UI Example</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jqueryui/1.13.2/jquery-ui.min.js" integrity="sha512-57oZ/vW8ANMjR/KQ6Be9v/+/h6bq9/l3f0Oc7vn6qMqyhvPd1cvKBRWWpzu0QoneImqr2SkmO4MSqU+RpHom3Q==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/1.1.3/axios.min.js"></script>
  </head>
  <body>
    <div class="info-vehi-details" id="vehicleDetails">
      <p class="vehi-name">
        <span class="label">Name:</span>
        <span class="value">Adder</span>
      </p>
      <p class="vehi-class">
        <span class="label">Class:</span>
        <span class="value">Super</span>
      </p>
      <p class="vehi-model">
        <span class="label">Model:</span>
        <span class="value">adder</span>
      </p>
      <p class="vehi-plate">
        <span class="label">Plate:</span>
        <span class="value">testname</span>
      </p>
    </div>
    <script src="script.js"></script>
  </body>
</html>
```

**JavaScript**

```js
// script.js
window.addEventListener("message", (event) => {
    if (event.data.type === "openmenu") {

		// Update vehicle details in UI
		$('.info-vehi-details').fadeIn(); // Show also you can use body fade
		$('#vehicleDetails .vehi-name .value').text(data.vehiclename);
		$('#vehicleDetails .vehi-class .value').text(data.vehicleclass);
		$('#vehicleDetails .vehi-model .value').text(data.vehiclemodel);
		$('#vehicleDetails .vehi-plate .value').text(data.vehicleplate);
    }
});

```
When the `/openui` command is executed, the vehicle details will be displayed in the UI.

## **2. Closing the UI**

**Lua Script: Sending Close Command**

```lua
-- Lua Script (FiveM)
RegisterCommand('closeui', function()
    SendNUIMessage({
        type = "closemenu" -- Unique identifier for closing
    })
end)

```

**JavaScript**

```js
// script.js
window.addEventListener("message", (event) => {
    if (event.data.type === "closemenu") {
        document.getElementById("vehicleDetails").style.display = "none";
    }
});
```

Okay now we have completed, Send fivem data to Ui side part, Let go to Get data from Ui side and pass to fivem side

## **3. Sending Data from the UI to FiveM**

### Example: Button to Toggle Vehicle Lights

**HTML: Adding a Button**

```html
<button id="toggleLights">Toggle Lights</button>
```

**JavaScript: Capturing Button Click and Sending Data**

```js
document.getElementById("toggleLights").addEventListener("click", () => {
	$.post(`https://${GetParentResourceName()}/toggleLights`, JSON.stringify({action: "toggle"})); // You should use jquery for this
});
```

**Lua Script**

```lua
RegisterNUICallback("toggleLights", function(data, cb)
    if data.action == "toggle" then
        -- Perform action in FiveM, e.g., toggle vehicle lights
        print("Vehicle lights toggled!")
    end
    cb("ok")
end)
```

## **4. Best Practices**

### Recommended Libraries
To ensure compatibility with FiveM: Also recommend to use those libraries because some js functions are not supported on fivem

```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jqueryui/1.13.2/jquery-ui.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/axios/1.1.3/axios.min.js"></script>
```

### Debugging Tips

- Use `SendNUIMessage` to test sending data from Lua to UI.
- Use browser developer tools to inspect the UI and debug JavaScript.
- Add console logs in Lua and JavaScript to verify data.

This guide should provide you with a solid foundation for creating interactive and dynamic UIs for your FiveM servers.

If you found this guide helpful, please show your support by **starring the repository** 

Thank you! 😊
