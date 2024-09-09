<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Travel Planning System</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .container { max-width: 800px; margin: auto; }
        .section { margin-bottom: 20px; }
        label { display: block; margin-top: 10px; }
        select, input { width: 100%; padding: 8px; margin-top: 5px; }
        .place, .travel-plan { border: 1px solid #ddd; padding: 10px; margin-top: 10px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Travel Planning System</h1>
        <div class="section">
            <label for="city">Select Destination City:</label>
            <select id="city">
                <option value="">Select a city</option>
                <option value="Vadodara">Vadodara</option>
                <option value="Surat">Surat</option>
                <option value="Ahmedabad">Ahmedabad</option>
                <option value="Rajkot">Rajkot</option>
                <option value="Gandhinagar">Gandhinagar</option>
            </select>
        </div>
        <div class="section">
            <label for="placeType">Select Type of Places:</label>
            <select id="placeType">
                <option value="">Select a type</option>
                <!-- Options will be populated dynamically -->
            </select>
        </div>
        <div class="section">
            <label for="budget">Enter Your Budget (INR per night):</label>
            <input type="number" id="budget" min="0" />
        </div>
        <div class="section">
            <label for="duration">Enter Trip Duration (days):</label>
            <input type="number" id="duration" min="1" />
        </div>
        <button onclick="generatePlan()">Generate Plan</button>
        
        <div id="placesList" class="section">
            <h2>Places to Visit:</h2>
            <!-- Places will be displayed here -->
        </div>
        
        <div id="travelPlan" class="section">
            <h2>Suggested Travel Plan:</h2>
            <!-- Travel plan will be displayed here -->
        </div>
    </div>

    <script>
        const data = {
            Vadodara: {
                Historical: [
                    { name: 'Laxmi Vilas Palace', description: 'A grand palace with beautiful architecture.' },
                    { name: 'Baroda Museum', description: 'Museum showcasing historical artifacts.' }
                ],
                Entertainment: [
                    { name: 'Sayaji Garden', description: 'A large park with recreational activities.' },
                    { name: 'Inorbit Mall', description: 'A modern shopping mall with entertainment options.' }
                ],
                Religious: [
                    { name: 'Kirti Mandir', description: 'A significant temple dedicated to Lord Krishna.' },
                    { name: 'Sardar Patel Planetarium', description: 'A planetarium that offers educational shows.' }
                ],
                Nature: [
                    { name: 'Narmada Canal', description: 'A scenic canal with beautiful surroundings.' },
                    { name: 'Gopi Talav', description: 'A picturesque lake with lush greenery.' }
                ]
            },
            Surat: {
                Historical: [
                    { name: 'Sardar Patel Museum', description: 'Museum dedicated to Sardar Patel.' },
                    { name: 'Surat Fort', description: 'Historic fort with significant past.' }
                ],
                Entertainment: [
                    { name: 'Dumas Beach', description: 'Popular beach with numerous activities.' },
                    { name: 'Surat Adventure Park', description: 'Park with adventure and fun activities.' }
                ],
                Religious: [
                    { name: 'Swaminarayan Temple', description: 'A prominent temple with spiritual significance.' },
                    { name: 'Jama Masjid', description: 'An ancient mosque with intricate designs.' }
                ],
                Nature: [
                    { name: 'Sarthana Nature Park', description: 'A park with diverse flora and fauna.' },
                    { name: 'Gopi Talav', description: 'A serene lake surrounded by nature.' }
                ]
            },
            Ahmedabad: {
                Historical: [
                    { name: 'Sabarmati Ashram', description: 'Historic ashram of Mahatma Gandhi.' },
                    { name: 'Jama Masjid', description: 'A grand mosque with rich history.' }
                ],
                Entertainment: [
                    { name: 'Kankaria Lake', description: 'A large lake with various entertainment options.' },
                    { name: 'Adalaj Stepwell', description: 'A beautiful stepwell with intricate carvings.' }
                ],
                Religious: [
                    { name: 'Swaminarayan Akshardham', description: 'A major temple with stunning architecture.' },
                    { name: 'Jama Masjid', description: 'A historic mosque known for its architectural beauty.' }
                ],
                Nature: [
                    { name: 'Kamla Nehru Zoo', description: 'A zoo with a variety of animals.' },
                    { name: 'Indroda Nature Park', description: 'A large nature park with diverse wildlife.' }
                ]
            },
            Rajkot: {
                Historical: [
                    { name: 'Rajkot Fort', description: 'A historical fort with significant past.' },
                    { name: 'Watson Museum', description: 'Museum showcasing local history and culture.' }
                ],
                Entertainment: [
                    { name: 'Rani Ki Vav', description: 'An ancient stepwell with historical importance.' },
                    { name: 'Funworld', description: 'A popular amusement park with various rides.' }
                ],
                Religious: [
                    { name: 'Swaminarayan Temple', description: 'A prominent temple with spiritual significance.' },
                    { name: 'Jain Temple', description: 'A historic Jain temple with intricate carvings.' }
                ],
                Nature: [
                    { name: 'Sardar Patel Planetarium', description: 'A planetarium offering educational shows.' },
                    { name: 'Aji Dam Garden', description: 'A serene garden near Aji Dam.' }
                ]
            },
            Gandhinagar: {
                Historical: [
                    { name: 'Gandhinagar Temple', description: 'An ancient temple with historical significance.' },
                    { name: 'Khadi Gramodyog Bhavan', description: 'A center for Khadi and rural crafts.' }
                ],
                Entertainment: [
                    { name: 'Indroda Nature Park', description: 'A large park with diverse wildlife.' },
                    { name: 'Gandhinagar Water Park', description: 'A fun water park with various rides.' }
                ],
                Religious: [
                    { name: 'Akshardham Temple', description: 'A magnificent temple with intricate carvings.' },
                    { name: 'Parmeshwar Temple', description: 'A significant temple with spiritual importance.' }
                ],
                Nature: [
                    { name: 'Gandhinagar Botanical Garden', description: 'A beautiful garden with a variety of plants.' },
                    { name: 'Sarita Udyan', description: 'A large park with lush greenery and walking paths.' }
                ]
            }
        };

        document.getElementById('city').addEventListener('change', function() {
            const city = this.value;
            const placeTypeDropdown = document.getElementById('placeType');
            placeTypeDropdown.innerHTML = '<option value="">Select a type</option>';
            if (city) {
                const placeTypes = Object.keys(data[city]);
                placeTypes.forEach(type => {
                    placeTypeDropdown.innerHTML += `<option value="${type}">${type}</option>`;
                });
            }
        });

        function generatePlan() {
            const city = document.getElementById('city').value;
            const type = document.getElementById('placeType').value;
            const budget = parseInt(document.getElementById('budget').value, 10);
            const duration = parseInt(document.getElementById('duration').value, 10);

            const placesList = document.getElementById('placesList');
            const travelPlan = document.getElementById('travelPlan');

            if (city && type && budget > 0 && duration > 0) {
                // Display places
                const places = data[city][type];
                placesList.innerHTML = `<h2>Places to Visit in ${city} (${type}):</h2>`;
                places.forEach(place => {
                    placesList.innerHTML += `<div class="place"><strong>${place.name}</strong><p>${place.description}</p></div>`;
                });

                // Generate travel plan
                const dailyBudget = budget / duration;
                travelPlan.innerHTML = `<h2>Suggested Travel Plan:</h2><p>Your daily budget for accommodation is ₹${dailyBudget.toFixed(2)}. Based on your preferences, you can explore the above places within your budget and duration.</p>`;
            } else {
                places
