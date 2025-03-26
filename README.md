<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nauka słówek</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .container { margin-top: 50px; }
        input, button { margin: 10px; padding: 5px; }
        .correct { border: 2px solid green; }
        .incorrect { border: 2px solid red; }
        .suggestion { color: orange; font-size: 14px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Nauka słówek</h1>
        <label for="questionCount">Liczba pytań:</label>
        <input type="number" id="questionCount" min="1" value="5">
        <br>
        <label for="direction">Kierunek tłumaczenia:</label>
        <select id="direction">
            <option value="pl-en">Polski -> Angielski</option>
            <option value="en-pl">Angielski -> Polski</option>
        </select>
        <br>
        <button onclick="startQuiz()">Rozpocznij</button>
        <div id="quiz"></div>
        <button onclick="checkAnswers()">Sprawdź</button>
        <div id="result"></div>
    </div>

    <script>
        const words = {
            "activity holiday": "wakacje ze zorganizowanymi zajęciami",
            "beach holiday": "wakacje na plaży",
            "breathtaking view": "widok zapierający dech",
            "camping holiday": "wakacje pod namiotem",
            "city break": "krótkie wakacje spędzone w mieście",
            "crowded beaches": "zatłoczone plaże",
            "cruise": "rejs wycieczkowy",
            "delicious food": "pyszne jedzenie",
            "downside": "wada, minus",
            "fine weather": "ładna, bezdeszczowa pogoda",
            "foreign travel": "podróże zagraniczne",
            "go backpacking": "jechać z plecakiem",
            "go hitchhiking": "podróżować autostopem",
            "go kayaking": "jechać na kajaki",
            "go on holiday": "jechać na wakacje",
            "go sightseeing": "zwiedzać",
            "go snorkelling": "nurkować z rurką do oddychania",
            "go swimming": "pływać",
            "go trekking": "wędrować",
            "highlight": "najlepsze wydarzenie",
            "hike": "piesza wędrówka, wędrować",
            "holiday resort": "kurort",
            "journey": "podróż z jednego miejsca do drugiego",
            "lose": "zgubić",
            "memory": "wspomnienie",
            "package holiday": "wczasy zorganizowane",
            "pebble beach": "kamienista plaża",
            "pollution": "zanieczyszczenie",
            "safari": "safari",
            "sandy beach": "piaszczysta plaża",
            "sea view": "widok na morze",
            "see sb off": "odprowadzać kogoś",
            "set up camp": "rozbijać obóz",
            "sleep in a tent": "spać w namiocie",
            "souvenir": "pamiątka",
            "sunbathe": "opalać się",
            "sunny weather": "słoneczna pogoda",
            "take photos": "robić zdjęcia",
            "tourism": "turystyka",
            "travel": "podróż, podróżowanie",
            "trip": "wycieczka, krótka lub dłuższa podróż",
            "upside": "zaleta, plus",
            "visitor": "zwiedzający, gość",
            "watch the wildlife": "obserwować dziką przyrodę",
            "weather": "pogoda","accommodation": "zakwaterowanie",
    "B&B": "pokoje gościnne ze śniadaniem",
    "bed and breakfast": "pokoje gościnne ze śniadaniem",
    "campsite": "pole kempingowe",
    "caravan": "przyczepa kempingowa",
    "chalet": "domek letniskowy",
    "family-run hotel": "rodzinny hotel",
    "five-star hotel": "hotel pięciogwiazdkowy",
    "guest house": "pensjonat",
    "luxury hotel": "luksusowy hotel",
    "self-catering apartment": "mieszkanie z wyżywieniem we własnym zakresie",
    "tent": "namiot",
    "villa": "letni dom do wynajęcia",
    "youth hostel": "schronisko młodzieżowe",
    "air conditioning": "klimatyzacja",
    "book a room": "rezerwować pokój",
    "check in": "zameldować się w hotelu",
    "check out": "wymeldować się z hotelu",
    "double room": "dwuosobowy pokój",
    "en-suite bathroom": "pokój z łazienką",
    "guest": "gość",
    "including breakfast": "zawierający śniadanie w cenie",
    "reception": "recepcja",
    "room service": "obsługa pokojowa",
    "single room": "pokój jednoosobowy",
    "stay at a hotel": "zatrzymać się w hotelu",
    "twin room": "dwuosobowy pokój z dwoma łóżkami",
    "vacancy": "wolny pokój",
    "Wi-Fi": "internet przewodowy",
    "across the street": "przez ulicę",
    "along the river": "wzdłuż rzeki",
    "ask the way": "zapytać o drogę",
    "show the way": "pokazać drogę",
    "cross the bridge": "przejść przez most",
    "get lost": "zgubić się",
    "go north": "iść na północ",
    "go south": "iść na południe",
    "go east": "iść na wschód",
    "go west": "iść na zachód",
    "go past": "przejść obok",
    "go straight": "iść prosto",
    "in the north": "na północy",
    "in the south": "na południu",
    "in the east": "na wschodzie",
    "in the west": "na zachodzie",
    "on the corner": "na rogu",
    "on your left / right": "po lewej / prawej",
    "take the second turning on the left": "skręcić w drugą ulicę w lewo",
    "towards": "w kierunku",
    "turn left / right": "skręć w lewo / prawo",
    "turn round": "zawrócić",
    "airport": "lotnisko",
    "aisle seat": "miejsce przy wejściu",
    "arrival": "przyjazd, przylot",
    "arrive at the airport": "przyjechać na lotnisko",
    "arrive in London": "przyjechać do Londynu",
    "backpack": "plecak",
    "baggage collection area": "odbiór bagażu",
    "board the plane": "wejść do samolotu",
    "boarding pass": "karta pokładowa",
    "book a low-cost ticket": "zarezerwować lot tanimi liniami",
    "catch a flight": "polecieć",
    "depart from": "odjeżdżać z, wyruszać z",
    "departure": "odjazd, odlot",
    "destination": "miejsce docelowe",
    "disembark": "zejść z pokładu",
    "drive a coach": "prowadzić autokar",
    "drive a lorry": "prowadzić ciężarówkę",
    "driver": "kierowca",
    "duty-free zone": "strefa bezcłowa",
    "fare": "opłata za przejazd",
    "fasten your seatbelt": "zapiąć pasy",
    "ferry": "prom",
    "fly a plane": "pilotować samolot",
    "fly on a plane": "lecieć samolotem",
    "get off the plane": "wysiadać z samolotu",
    "get on the plane": "wsiadać do samolotu",
    "get stuck in traffic": "utknąć w korku",
    "go by boat": "płynąć łodzią",
    "go by bus": "jechać autobusem",
    "go by plane": "lecieć samolotem",
    "go by rail": "podróżować koleją/pociągiem",
    "go by sea": "podróżować drogą morską",
    "go by train": "podróżować koleją, pociągiem",
    "go for a ride": "wybrać się na przejażdżkę",
    "go on a cruise": "wyruszyć w rejs",
    "go on an excursion": "wybrać się na wycieczkę",
    "go on foot": "iść pieszo",
    "go through customs": "przejść odprawę celną",
    "hand luggage": "bagaż podręczny",
    "itinerary": "trasa, lista miejsc do odwiedzenia",
    "jet off": "odlecieć",
    "jet lag": "zmęczenie spowodowane zmianą stref czasowych podczas podróży",
    "land": "lądować",
    "luggage": "bagaż",
    "means of transport": "środek transportu",
    "miss the bus/train": "spóźnić się na autobus/pociąg",
    "miss the flight/plane": "spóźnić się na lot/samolot",
    "on-board service": "obsługa na pokładzie samolotu",
    "overhead locker": "szafka pod sufitem samolotu",
    "passenger": "pasażer/pasażerka",
    "passport": "paszport",
    "pilot": "pilot",
    "platform": "peron",
    "priority check-in": "odprawa priorytetowa",
    "return ticket": "bilet w obie strony",
    "ride a bike": "jeździć rowerem/motocyklem",
    "rucksack": "plecak",
    "single ticket": "bilet w jedną stronę",
    "station": "stacja, dworzec",
    "suitcase": "walizka",
    "take off": "startować",
    "taxi rank": "postój taksówek",
    "ticket office": "kasa biletowa",
    "timetable": "rozkład jazdy, rozkład lotów",
    "tourist guide": "przewodnik/przewodniczka",
    "travel around the world": "podróżować dookoła świata",
    "travel by boat": "płynąć łodzią",
    "travel by bus": "jechać autobusem",
    "travel by plane": "lecieć samolotem",
    "travel by rail/train": "podróżować koleją/pociągiem",
    "travel by sea": "podróżować drogą morską",
    "turbulence": "turbulencje",
    "underground": "metro",
    "van": "furgonetka",
    "vehicle": "pojazd",
    "window seat": "miejsce przy oknie",
    "brake": "hamować",
    "lane": "pas jezdni",
    "one-way street": "ulica jednokierunkowa",
    "parking space": "miejsce parkingowe",
    "pavement": "chodnik",
    "pedestrian": "pieszy",
    "pedestrian zone": "strefa ruchu pieszego",
    "road signs": "znaki drogowe",
    "rush/peak hour": "godziny szczytu",
    "speed limit": "ograniczenie prędkości",
    "stop light": "czerwone światło",
    "traffic congestion": "natężenie ruchu",
    "traffic jam": "korek uliczny",
    "traffic lights": "sygnalizacja świetlna",
    "traffic": "ruch uliczny",
    "zebra crossing": "przejście dla pieszych",
    "be knocked down": "zostać potrąconym",
    "be seasick": "mieć chorobę morską",
    "break down": "zepsuć się",
    "cancel a flight": "odwołać lot",
    "cancellation": "odwołanie lotu",
    "car crash": "wypadek samochodowy",
    "train crash": "wypadek kolejowy",
    "delayed": "opóźniony",
    "double-booked": "podwójnie zarezerwowany",
    "fall off a bike": "spaść z roweru",
    "food poisoning": "zatrucie pokarmowe",
    "have technical problems": "mieć problemy techniczne",
    "lose control of a vehicle": "stracić kontrolę nad pojazdem",
    "road accident": "wypadek drogowy",
    "sink": "tonąć",
    "swerve across the road": "gwałtownie zjechać z drogi",
    "keep an eye on your belongings": "pilnować bagażu",
    "emergency cash": "pieniądze na wypadek sytuacji awaryjnej",
    "lose travel documents": "zgubić dokumenty podróżne",
    "emergency information": "dane kontaktowe w razie sytuacji awaryjnej",
    "insect repellent": "środek na owady",
    "scam": "przekręt",
    "sunstroke": "udar słoneczny",
    "vaccinations": "szczepienia"
        };

        function startQuiz() {
            const count = document.getElementById("questionCount").value;
            const direction = document.getElementById("direction").value;
            const quizDiv = document.getElementById("quiz");
            quizDiv.innerHTML = "";
            
            const keys = Object.keys(words);
            for (let i = 0; i < count; i++) {
                const randomWord = keys[Math.floor(Math.random() * keys.length)];
                const question = direction === "pl-en" ? randomWord : words[randomWord];
                quizDiv.innerHTML += `<p>${question} <input type='text' data-answer='${direction === "pl-en" ? words[randomWord] : randomWord}'></p>`;
            }
        }
        
        function checkAnswers() {
            let correct = 0;
            const inputs = document.querySelectorAll("#quiz input");
            inputs.forEach(input => {
                const answer = input.dataset.answer.toLowerCase().trim();
                const userAnswer = input.value.toLowerCase().trim();
                const suggestionSpan = document.createElement("span");
                suggestionSpan.classList.add("suggestion");

                if (userAnswer === answer) {
                    correct++;
                    input.classList.add("correct");
                    input.classList.remove("incorrect");
                    suggestionSpan.textContent = "";
                } else {
                    input.classList.add("incorrect");
                    input.classList.remove("correct");
                    suggestionSpan.textContent = ` Poprawna odpowiedź: ${answer}`;
                }
                input.parentNode.appendChild(suggestionSpan);
            });
            document.getElementById("result").innerText = `Poprawne odpowiedzi: ${correct} / ${inputs.length}`;
        }
    </script>
</body>
</html>
