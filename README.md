# Egg-opener-11
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Egg Opener Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f7f7f7;
            padding: 20px;
            overflow: hidden;
        }
        .button {
            padding: 15px 25px;
            font-size: 20px;
            color: #fff;
            background-color: #007BFF;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
        }
        .button:hover {
            background-color: #0056b3;
        }
        .output {
            margin-top: 20px;
            font-size: 24px;
            color: #333;
        }
        .animation-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            overflow: hidden;
        }
        .zoom-effect {
            animation: zoomOut 1s forwards, zoomIn 1s 1.2s forwards;
        }
        .pet-popup {
            position: absolute;
            background-color: white;
            border: 2px solid #ccc;
            border-radius: 10px;
            padding: 20px;
            text-align: center;
            opacity: 0;
            animation: fadeIn 1s 2.5s forwards;
            font-size: 36px;
            font-weight: bold;
            color: #333;
        }
        @keyframes zoomOut {
            from {
                transform: scale(1);
            }
            to {
                transform: scale(0.5);
            }
        }
        @keyframes zoomIn {
            from {
                transform: scale(0.5);
            }
            to {
                transform: scale(1);
            }
        }
        @keyframes fadeIn {
            from {
                opacity: 0;
            }
            to {
                opacity: 1;
            }
        }
    </style>
</head>
<body>
    <h1>Egg Opener Simulator</h1>
    <div>
        <button id="inventoryButton" class="button" style="margin-right: auto; float: left;" onclick="toggleInventory()">Inventar</button>
        <button id="openButton" class="button" onclick="openEgg()">Open Egg</button>
    </div>
    <div class="output" id="output">Click the button to open an egg!</div>
    <div id="popupContainer"></div>

    <!-- Animation Container -->
    <div class="animation-container" id="animationContainer">
        <div class="pet-popup" id="petPopup">Rare Fox!</div>
    </div>

    <script>
        // Pets and their probabilities
        const pets = [
            { name: "Common Cat", chance: 60.0, count: 0 },
            { name: "Common Dog", chance: 20.0, count: 0 },
            { name: "Uncommon Bird", chance: 10.0, count: 0 },
            { name: "Rare Fox", chance: 5.0, count: 0 },
            { name: "Epic Dragon", chance: 2.5, count: 0 },
            { name: "Legendary Unicorn", chance: 1.0, count: 0 },
            { name: "Mythic Phoenix", chance: 0.5, count: 0 },
            { name: "Divine Tiger", chance: 0.25, count: 0 },
            { name: "Godly Wolf", chance: 0.1, count: 0 },
            { name: "Celestial Whale", chance: 0.05, count: 0 },
        ];

        // Calculate total chance
        const totalChance = pets.reduce((sum, pet) => sum + pet.chance, 0);

        // Function to draw a random pet
        function openEgg() {
            const random = Math.random() * totalChance;
            let cumulativeChance = 0;

            for (const pet of pets) {
                cumulativeChance += pet.chance;
                if (random <= cumulativeChance) {
                    pet.count++;
                    if (pet.name === "Rare Fox") {
                        playRareAnimation(pet.name);
                    } else {
                        showPopup(pet.name);
                    }
                    return;
                }
            }
        }

        // Function to display popup with animation
        function showPopup(petName) {
            const popupContainer = document.getElementById("popupContainer");

            // Clear previous popup
            popupContainer.innerHTML = "";

            // Create popup element
            const popup = document.createElement("div");
            popup.className = "pet-popup";
            popup.textContent = `You got: ${petName}`;

            // Add popup to container
            popupContainer.appendChild(popup);

            // Remove popup after 3 seconds
            setTimeout(() => {
                popup.remove();
            }, 3000);
        }

        // Play rare animation for "Rare Fox"
        function playRareAnimation(petName) {
            const animationContainer = document.getElementById("animationContainer");
            const petPopup = document.getElementById("petPopup");
            const openButton = document.getElementById("openButton");
            const inventoryButton = document.getElementById("inventoryButton");

            // Disable buttons
            openButton.disabled = true;
            inventoryButton.disabled = true;

            // Show animation container and add animation classes
            animationContainer.style.display = "flex";
            petPopup.textContent = petName;
            animationContainer.classList.add("zoom-effect");

            // Remove animation after completion and enable buttons
            setTimeout(() => {
                animationContainer.style.display = "none";
                animationContainer.classList.remove("zoom-effect");
                openButton.disabled = false;
                inventoryButton.disabled = false;
            }, 3500); // Total animation time
        }
    </script>
</body>
</html>
