Relógio com Calendário

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calend�rio e Rel�gio</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
        }
        .calendar {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        .calendar h2 {
            text-align: center;
            margin: 0 0 10px;
        }
        .calendar table {
            border-collapse: collapse;
            width: 100%;
        }
        .calendar th, .calendar td {
            text-align: center;
            padding: 10px;
            border: 1px solid #ddd;
        }
        .calendar th {
            background-color: #f2f2f2;
        }
        .today {
            background-color: #007bff;
            color: white;
            border-radius: 50%;
        }
        .clock-container {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .analog-clock {
            width: 200px;
            height: 200px;
            background-color: white;
            border-radius: 50%;
            position: relative;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            margin-bottom: 20px;
            border: 3px solid #333; /* Borda no rel�gio */
        }
        .hand {
            position: absolute;
            bottom: 50%;
            left: 50%;
            transform-origin: bottom;
            background-color: black;
        }
        .hour-hand {
            width: 4px;
            height: 60px;
        }
        .minute-hand {
            width: 3px;
            height: 80px;
        }
        .second-hand {
            width: 2px;
            height: 90px;
            background-color: red;
        }
        .center {
            width: 10px;
            height: 10px;
            background-color: black;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        .number {
            position: absolute;
            font-size: 16px;
            font-weight: bold;
            color: #333;
        }
        .number-12 { top: 10px; left: 50%; transform: translateX(-50%); }
        .number-3 { right: 10px; top: 50%; transform: translateY(-50%); }
        .number-6 { bottom: 10px; left: 50%; transform: translateX(-50%); }
        .number-9 { left: 10px; top: 50%; transform: translateY(-50%); }
        .digital-clock {
            font-size: 24px;
            background-color: white;
            padding: 10px 20px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <div class="calendar" id="calendar"></div>
    <div class="clock-container">
        <div class="analog-clock">
            <div class="hand hour-hand" id="hour"></div>
            <div class="hand minute-hand" id="minute"></div>
            <div class="hand second-hand" id="second"></div>
            <div class="center"></div>
            <div class="number number-12">12</div>
            <div class="number number-3">3</div>
            <div class="number number-6">6</div>
            <div class="number number-9">9</div>
        </div>
        <div class="digital-clock" id="digital-clock"></div>
    </div>

    <script>
        // Calend�rio
        function generateCalendar() {
            const today = new Date();
            const year = today.getFullYear();
            const month = today.getMonth();
            const currentDay = today.getDate();
            const firstDay = new Date(year, month, 1).getDay();
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            
            const monthNames = [
                "Janeiro", "Fevereiro", "Mar�o", "Abril", "Maio", "Junho",
                "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"
            ];
            
            let calendarHTML = `
                <h2>${monthNames[month]} ${year}</h2>
                <table>
                    <tr>
                        <th>Dom</th><th>Seg</th><th>Ter</th><th>Qua</th><th>Qui</th><th>Sex</th><th>S�b</th>
                    </tr>
                    <tr>
            `;
            
            let day = 1;
            for (let i = 0; i < 6; i++) {
                for (let j = 0; j < 7; j++) {
                    if (i === 0 && j < firstDay) {
                        calendarHTML += '<td></td>';
                    } else if (day <= daysInMonth) {
                        if (day === currentDay) {
                            calendarHTML += `<td class="today">${day}</td>`;
                        } else {
                            calendarHTML += `<td>${day}</td>`;
                        }
                        day++;
                    }
                }
                calendarHTML += '</tr><tr>';
            }
            calendarHTML += '</tr></table>';
            document.getElementById('calendar').innerHTML = calendarHTML;
        }

        // Rel�gio Anal�gico e Digital
        function updateClock() {
            const now = new Date();
            const hours = now.getHours();
            const minutes = now.getMinutes();
            const seconds = now.getSeconds();

            // Rel�gio Anal�gico
            const hourDeg = (hours % 12 + minutes / 60) * 30;
            const minuteDeg = (minutes + seconds / 60) * 6;
            const secondDeg = seconds * 6;

            document.getElementById('hour').style.transform = `rotate(${hourDeg}deg)`;
            document.getElementById('minute').style.transform = `rotate(${minuteDeg}deg)`;
            document.getElementById('second').style.transform = `rotate(${secondDeg}deg)`;

            // Rel�gio Digital
            const timeString = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            document.getElementById('digital-clock').textContent = timeString;
        }

        // Inicializar
        generateCalendar();
        updateClock();
        setInterval(updateClock, 1000);
    </script>
</body>
</html>
