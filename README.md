# XEGGEX-Faucet-Autoclame-2025
ab 01.03.2025
auto update 

function claimAll() {
    var buttons = document.querySelectorAll(".btn.btn-sm.makeclaim:not([disabled])"); // Pobierz dostępne przyciski
    if (buttons.length === 0) {
        console.log("Brak dostępnych claimów, sprawdzam ponownie później...");
        checkTimers();
        return;
    }

    function clickButtonsSequentially(index) {
        if (index < buttons.length) {
            buttons[index].click();
            console.log(`Kliknięto przycisk ${index + 1}`);
            setTimeout(() => clickButtonsSequentially(index + 1), 1000); // 1s delay
        }
    }

    clickButtonsSequentially(0);
}

// 🔹 Sprawdza czas do następnego claimowania i ustawia automatyczne odświeżenie
function checkTimers() {
    var timers = document.querySelectorAll(".makeclaim + span"); // Znajduje timery obok przycisku Claim
    var minTime = Infinity;

    timers.forEach(timer => {
        let timeParts = timer.innerText.match(/(\d+):(\d+)/); // Szuka formatu MM:SS
        if (timeParts) {
            let minutes = parseInt(timeParts[1], 10);
            let seconds = parseInt(timeParts[2], 10);
            let totalSeconds = minutes * 60 + seconds;

            if (totalSeconds < minTime) {
                minTime = totalSeconds;
            }
        }
    });

    if (minTime !== Infinity) {
        console.log(`Najkrótszy czas do claim: ${Math.floor(minTime / 60)} min ${minTime % 60} sek.`);
        setTimeout(claimAll, (minTime + 1) * 1000); // Poczekaj na najbliższy claim i uruchom ponownie
    }
}

// 🔹 Uruchom claimowanie i cykliczne sprawdzanie
claimAll();
setInterval(checkTimers, 30000); // Sprawdza co 30 sekund
